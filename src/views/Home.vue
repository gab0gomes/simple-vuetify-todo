<template>
  <v-container>
    <v-layout>
      <v-flex xs12 sm8 offset-sm2>
        <v-card>
          <v-card-title primary-title>
            <h3 class="headline mb-0">Coisas a fazer</h3>
          </v-card-title>
          <v-card-text>
            <v-alert
              :value="todoList.length === 0"
              color="success"
              icon="check_circle"
              outline
              transition="scale-transition"
              class="mb-3"
            >Nada a fazer.</v-alert>

            <v-list v-show="todoList.length > 0" two-line>
              <v-list-tile v-for="todo in todoList" :key="todo.id">
                <v-list-tile-action>
                  <v-checkbox></v-checkbox>
                </v-list-tile-action>

                <v-list-tile-content>
                  <v-list-tile-title>{{ todo.title }}</v-list-tile-title>
                  <v-list-tile-sub-title>
                      {{ `${todo.date} às ${todo.time}h` }}
                  </v-list-tile-sub-title>
                </v-list-tile-content>
              </v-list-tile>
            </v-list>
          </v-card-text>

          <v-card-actions>
            <v-btn
              absolute
              dark
              fab
              bottom
              right
              color="light-blue"
              @click.stop="showDialog = true"
            >
              <v-icon>add</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
    <add-todo-dialog
        :dialog="showDialog"
        @dismiss="showDialog = false"
        @success="createTodo"
    ></add-todo-dialog>
  </v-container>
</template>

<script>
import addTodoDialog from '../components/add-todo-dialog.vue';

export default {
  components: {
    addTodoDialog,
  },

  data() {
    return {
      showDialog: false,
      todoList: [],
    };
  },

  methods: {
    createTodo(todoData) {
      this.showDialog = false;
      this.todoList.push({
        id: Date.now(),
        ...todoData,
      });
    },
  },
};
</script>

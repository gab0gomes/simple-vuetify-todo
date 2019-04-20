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
              <template v-for="(todo, index) in todoList">
                <v-list-tile :key="todo.id">
                  <v-list-tile-action>
                    <v-checkbox color="teal" @change="toggleDone($event, todo.id)"></v-checkbox>
                  </v-list-tile-action>

                  <v-list-tile-content>
                    <v-list-tile-title>
                      <s v-if="todo.done">{{ todo.title }}</s>
                      <p v-else>{{ todo.title }}</p>
                    </v-list-tile-title>
                    <v-list-tile-sub-title>{{ `${todo.date} às ${todo.time}h` }}</v-list-tile-sub-title>
                  </v-list-tile-content>

                  <v-list-tile-action>
                    <v-btn flat icon color="grey" @click="removeTodo(todo.id)">
                      <v-icon>delete</v-icon>
                    </v-btn>
                  </v-list-tile-action>
                </v-list-tile>
                <v-divider v-if="index + 1 < todoList.length" :key="index"></v-divider>
              </template>
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
    <add-todo-dialog :dialog="showDialog" @dismiss="showDialog = false" @success="createTodo"></add-todo-dialog>
  </v-container>
</template>

<script>
import _ from "lodash";

import addTodoDialog from "../components/add-todo-dialog.vue";

export default {
  components: {
    addTodoDialog
  },

  data() {
    return {
      showDialog: false,
      todoList: []
    };
  },

  methods: {
    createTodo(todoData) {
      this.showDialog = false;
      this.todoList.push({
        id: Date.now(),
        done: false,
        ...todoData
      });
    },
    removeTodo(id) {
      this.todoList = _.filter(this.todoList, function(todo) {
        return todo.id !== id;
      });
    },
    toggleDone(e, id) {
      let index = _.findIndex(this.todoList, function(todo) {
        return todo.id === id;
      });

      this.todoList[index].done = e;
    }
  }
};
</script>

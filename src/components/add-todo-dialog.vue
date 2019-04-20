<template>
  <div class="text-xs-center">
    <v-dialog v-model="showDialog" width="500" persistent>
      <v-card>
        <v-card-title class="title cyan darken-4 teal--text text--lighten-2" primary-title>Novo Todo</v-card-title>

        <v-card-text>
          <v-form ref="form">
            <v-container>
              <v-layout row wrap>
                <v-flex xs12>
                  <v-text-field v-model="title" label="Título" autofocus :rules="[rules.required]"></v-text-field>
                </v-flex>
                <v-flex xs12>
                  <v-menu
                    v-model="showDatePicker"
                    :close-on-content-click="false"
                    :nudge-right="40"
                    lazy
                    transition="scale-transition"
                    offset-y
                    full-width
                    max-width="290px"
                    min-width="290px"
                  >
                    <template v-slot:activator="{ on }">
                      <v-text-field
                        v-model="dateFormatted"
                        label="Data"
                        readonly
                        v-on="on"
                        :rules="[rules.required]"
                      ></v-text-field>
                    </template>
                    <v-date-picker v-model="date" no-title @input="showDatePicker = false"></v-date-picker>
                  </v-menu>
                </v-flex>
                <v-flex>
                  <v-menu
                    ref="menu"
                    v-model="showTimePicker"
                    :close-on-content-click="false"
                    :nudge-right="40"
                    :return-value.sync="time"
                    lazy
                    transition="scale-transition"
                    offset-y
                    full-width
                    max-width="290px"
                    min-width="290px"
                  >
                    <template v-slot:activator="{ on }">
                      <v-text-field
                        v-model="time"
                        label="Hora"
                        readonly
                        v-on="on"
                        :rules="[rules.required]"
                      ></v-text-field>
                    </template>
                    <v-time-picker
                      v-if="showTimePicker"
                      v-model="time"
                      full-width
                      @click:minute="$refs.menu.save(time)"
                      format="24hr"
                    ></v-time-picker>
                  </v-menu>
                </v-flex>
              </v-layout>
            </v-container>
          </v-form>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-btn color="red" flat @click="dismissDialog">Cancelar</v-btn>
          <v-spacer></v-spacer>
          <v-btn color="primary" flat @click="submitForm">Confirmar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: {
    dialog: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      title: null,
      showDialog: false,
      date: null,
      time: null,
      showDatePicker: false,
      showTimePicker: false,
      rules: {
        required: value => (!!value && !!value.trim()) || 'Campo obrigatório.'
      }
    };
  },

  computed: {
    dateFormatted: {
      get() {
        return this.formatDate(this.date);
      },
      set() {}
    },
    submitPacket() {
      return {
        date: this.dateFormatted,
        time: this.time,
        title: this.title
      };
    }
  },

  watch: {
    dialog() {
      this.showDialog = this.dialog;
    }
  },

  methods: {
    formatDate(date) {
      if (!date) return null;

      const [year, month, day] = date.split('-');
      return `${day}/${month}/${year}`;
    },
    parseDate(date) {
      if (!date) return null;

      const [day, month, year] = date.split('/');
      return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`;
    },
    submitForm() {
      if (this.$refs.form.validate()) {
        this.$emit('success', this.submitPacket);
        this.resetForm();
      }
    },
    resetForm() {
      this.$refs.form.reset();
    },
    dismissDialog() {
      this.$emit('dismiss');
      this.resetForm();
    }
  }
};
</script>

<style>
</style>

<template>
  <v-main class="list">
    <h3 class="text-h3 font-weight-medium mb-5">To Do List</h3>
    <v-card>
      <v-card-title>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
        <v-spacer></v-spacer>

        <v-btn color="success" dark @click="dialog = true"> Tambah </v-btn>
      </v-card-title>
      <v-data-table :headers="headers" :items="todos" :search="search">
        <template v-slot:[`item.priority`]="{ item }">
          <td>
            <v-chip v-if="item.priority == 'Penting'" color="red" outlined>
              {{ item.priority }}
            </v-chip>
            <v-chip
              v-else-if="item.priority == 'Biasa'"
              color="success"
              outlined
            >
              {{ item.priority }}
            </v-chip>
            <v-chip v-else color="primary" outlined>
              {{ item.priority }}
            </v-chip>
          </td>
        </template>

        <template v-slot:[`item.actions`]="{ item }">
          <v-btn small class="mr-2" @click="editItem(item)"> edit </v-btn>

          <v-btn small @click="deleteItem(item)"> delete </v-btn>
        </template>
      </v-data-table>
    </v-card>

    <v-dialog v-model="dialog" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline">Form Todo</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-text-field
              v-model="formTodo.task"
              label="Task"
              required
            ></v-text-field>

            <v-select
              v-model="formTodo.priority"
              :items="['Penting', 'Biasa', 'Tidak penting']"
              label="Priority"
              required
            ></v-select>

            <v-textarea
              v-model="formTodo.note"
              label="Note"
              required
            ></v-textarea>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="cancel"> Cancel </v-btn>

          <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialogDelete" max-width="500px">
      <v-card>
        <v-card-title class="headline"
          >Are you sure you want to delete this item?</v-card-title
        >
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
          <v-btn color="blue darken-1" text @click="deleteItemConfirm"
            >OK</v-btn
          >
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-main>
</template>

<script>
export default {
  name: "List",
  data() {
    return {
      search: null,
      dialog: false,
      headers: [
        {
          text: "Task",
          align: "start",
          sortable: true,
          value: "task",
        },
        { text: "Priority", value: "priority" },
        { text: "Note", value: "note" },
        { text: "Actions", value: "actions" },
      ],
      todos: [
        {
          task: "bernafas",
          priority: "Penting",
          note: "huffttt",
        },
        {
          task: "nongkrong",
          priority: "Tidak penting",
          note: "bersama tman2",
        },
        {
          task: "masak",
          priority: "Biasa",
          note: "masak air 500ml",
        },
      ],
      formTodo: {
        task: null,
        priority: null,
        note: null,
      },
    };
  },
  methods: {
    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.todos[this.editedIndex], this.formTodo);
      } else {
        this.todos.push(this.formTodo);
      }
      this.resetForm();
      this.dialog = false;
    },
    cancel() {
      this.resetForm();
      this.dialog = false;
    },
    editItem(item) {
      this.dialog = true;
      this.editedIndex = this.todos.indexOf(item);
      this.formTodo = Object.assign({}, item);
    },
    deleteItem(item) {
      this.editedIndex = this.todos.indexOf(item);
      this.formTodo = Object.assign({}, item);
      this.dialogDelete = true;
    },
    deleteItemConfirm() {
      this.todos.splice(this.editedIndex, 1);
      this.dialogDelete = false;
    },
    closeDelete() {
      this.dialogDelete = false;
    },
    resetForm() {
      this.formTodo = {
        task: null,
        priority: null,
        note: null,
      };
    },
  },
};
</script>
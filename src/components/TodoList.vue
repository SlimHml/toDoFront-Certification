<template>
  <div class="container">
    <form>
      <div class="input-group mb-3">
        <input
          v-model="input"
          @keydown.enter="addTodo()"
          id="output"
          type="text"
          class="form-control"
          placeholder="Ajouter une liste à faire"
          aria-label="Recipient's username"
          aria-describedby="basic-addon2"
        />
        <div class="input-group-append">
          <button
            @click="addTodo()"
            class="btn btn-outline-secondary"
            type="button"
            id="button-addon2"
          >Ajouter</button>
        </div>
      </div>
    </form>
    <div id="todo" v-for="todo in todos" v-bind:key="todo">
      <div class="modal" tabindex="-1" role="dialog">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">Changement d'avis?</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <input v-model="modalInput" placeholder="Editer votre todo ici" />
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
              <button
                v-for="todo in todos"
                v-bind:key="todo.id"
                @click="updateTodo(todo.id)"
                type="button"
                class="btn btn-primary"
              >Save changes {{todo.id}}</button>
            </div>
          </div>
        </div>
      </div>
      {{ todo.title }}
      <button
        @click="todoId(todo.id)"
        class="btn btn-info"
        data-toggle="modal"
        data-target=".modal"
      >Editer {{todo.id}}</button>
      <button @click="deleteTodo(todo.id)" class="btn btn-danger" type="button">X</button>

      <ul>
        <li v-for="item in todo.todoItems" v-bind:key="item">{{ item.content }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "TodoList",
  data() {
    return { todos: [], input: "", modalInput: "", title: "" };
  },

  methods: {
    async getAllTodos() {
      const response = await axios.get("http://localhost:4000/api/todos");
      this.todos = response.data;
    },

    addTodo() {
      axios.post("http://localhost:4000/api/todos", {
        title: this.input
      });
    },

    deleteTodo(id) {
      axios.delete(`http://localhost:4000/api/todos/${id}`);
    },

    updateTodo(id) {
      axios.patch(`http://localhost:4000/api/todos/${id}`, {
        title: this.modalInput
      });
    }
  },

  mounted() {
    this.getAllTodos();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
#output {
  max-width: 900px;
}
</style>

<template>
  <v-app class="hl">
    <div class="inside">
      <div class="d-flex justify-center">
        <h1 id="addTodo">Add ToDo Using Vue.Js and MongoDB</h1>
      </div>
      <form>
        <div class="d-flex inputdiv">
          <v-text-field
            bg-color="white"
            class="input"
            v-model="newTodo"
            label="Enter Title"
            required
            rounded
          ></v-text-field>

          <div>
            <v-btn class="btn mx-2" @click="addToDo()">Add ToDo</v-btn>
          </div>
        </div>
      </form>
      <!-- uncompleted todos -->
      <div class="">
        <h1 id="uncomplete">Uncompleted ToDo</h1>
      </div>
      <v-table>
        <thead>
          <tr>
            <th class="text-center">Title</th>

            <th class="text-center">Status</th>
            <th class="text-center">Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="todo in uncompletedTodos" :key="todo._id">
            <td>{{ todo.title }}</td>

            <td>
              <v-btn
                @click="completeTodo(todo._id)"
                class="mx-2"
                small
                color="green"
                >Done</v-btn
              >
            </td>
            <td>
              <v-btn
                @click="deleteTodo(todo._id)"
                class="mx-2"
                small
                color="red"
                >Delete</v-btn
              >
            </td>
          </tr>
        </tbody>
      </v-table>

      <!-- completed todos -->

      <div class="d-flex justify-center"></div>
      <h1 class="text-center completed">Completed Todos</h1>

      <v-table>
        <thead>
          <tr>
            <th class="text-center">Title</th>

            <th class="text-center">Status</th>
            <th class="text-center">Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="todo in completedTodos" :key="todo._id">
            <td>{{ todo.title }}</td>
            <td>
              <v-btn
                @click="uncompleteTodo(todo._id)"
                class="mx-2"
                small
                color="green"
                >UnDone</v-btn
              >
            </td>
            <td>
              <v-btn
                @click="deleteTodo(todo._id)"
                class="mx-2"
                small
                color="red"
                >Delete</v-btn
              >
            </td>
          </tr>
        </tbody>
      </v-table>
    </div>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data: () => ({
    newToDo: "",
    uncompletedTodos: [],
    completedTodos: [],
    url: "https://crud2-mysterious21pr.b4a.run",
  }),
  methods: {
    addToDo() {
      axios
        .post(this.url + "/todo/add", {
          todo: this.newTodo,
        })
        .then((response) => {
          console.log(response.data);
          this.created();
        });
      this.newTodo = "";
    },
    completeTodo(todoID) {
      axios.post(this.url + "/todo/complete/" + todoID).then((response) => {
        console.log(response.data);
        this.created();
      });
    },
    uncompleteTodo(todoID) {
      axios.post(this.url + "/todo/uncomplete/" + todoID).then((response) => {
        console.log(response.data);
        this.created();
      });
    },
    deleteTodo(todoID) {
      axios.delete(this.url + "/todo/" + todoID).then((response) => {
        console.log(response.data);
        this.created();
      });
    },
    created() {
      // fetch uncompleted task
      axios
        .get(this.url + "/todo/uncompleted")
        .then((response) => (this.uncompletedTodos = response.data))
        .catch((error) => console.log(error));
      // fetch completed task
      axios
        .get(this.url + "/todo/completed")
        .then((response) => (this.completedTodos = response.data))
        .catch((error) => console.log(error));
    },
  },
  created() {
    // fetch uncompleted task
    axios
      .get("https://crud2-mysterious21pr.b4a.run/todo/uncompleted")
      .then((response) => (this.uncompletedTodos = response.data))
      .catch((error) => console.log(error));
    // fetch completed task
    axios
      .get(this.url + "/todo/completed")
      .then((response) => (this.completedTodos = response.data))
      .catch((error) => console.log(error));
  },
};
</script>
<style scoped>
.btn {
  min-height: 55px;
  border-radius: 50px;
}
input {
  color: #1445b4;
  background-color: rgb(236, 240, 240);
}
.hl {
  background-image: url(https://images.unsplash.com/photo-1677784976154-816c3ceca511?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1856&q=80);
}
.inside {
  padding: 15px;
}
#addTodo {
  color: azure;
}
.completed {
  color: azure;
}
#uncomplete {
  color: azure;
}
</style>

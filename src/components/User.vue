<template>
  <div>
    <h1>List user : </h1>
    <ul>
      <li v-for="user in users" :key="user.id">
        {{ user.username }} <button @click="deleteUser(user.id)">-</button>
      </li>
    </ul>
    <div style="margin-bottom: 10px">
      <label for="" style="margin-right: 20px">Username</label>
      <input v-model="username" />
    </div>
    <div style="margin-bottom : 10px">
      <label for="" style="margin-right: 20px">Password </label>
      <input v-model="password" type="password" />
    </div>
    <button @click="addUser">Add User</button>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'app',
  data() {
    return {
      users: [],
      username: '',
      password: '',
    };
  },
  mounted: function () {
    this.getUsers();
  },
  methods: {
    getUsers() {
      const username = localStorage.getItem('usr');
      const password = localStorage.getItem('pwd');
      axios
        .get('http://localhost:3000/user', { headers: { username, password } })
        .then((result) => {
          this.users = result.data;
          console.log(result);
        });
    },
    addUser() {
      let addItem = {
        username: this.username,
        password: this.password,
      };
      const username = localStorage.getItem('usr');
      const password = localStorage.getItem('pwd');
      console.log(addItem);
      axios
        .post('http://localhost:3000/user', addItem, {
          headers: { username, password },
        })
        .then(() => {
          this.getUsers();
        });
    },
    deleteUser(id) {
      const username = localStorage.getItem('usr');
      const password = localStorage.getItem('pwd');
      axios
        .delete(`http://localhost:3000/user/${id}`, {
          headers: { username, password },
        })
        .then(() => {
          this.getUsers();
        });
    },
  },
};
</script>

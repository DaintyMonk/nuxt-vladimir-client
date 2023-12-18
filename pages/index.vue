<template>
  <div>
    <form @submit.prevent="submitForm">
      <label for="username">Username:</label>
      <input type="text" id="username" v-model="formData.username" required>

      <label for="password">Password:</label>
      <input type="password" id="password" v-model="formData.password" required>

      <button type="submit">Submit</button>
    </form>

    <table>
      <thead>
        <tr>
          <th>Пользователь</th>
          <th>Пароль</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user of users" :key="user.id">
          <td>{{user.username}}</td>
          <td>{{user.password}}</td>
        </tr>
      </tbody>
    </table>

    <button @click="updateTable">Обновить</button>
  </div>
</template>

<script>
export default {
  async asyncData({$axios}) {
    const users = await $axios.$get('/auth/get-users');
    return {users};
  },
  data() {
    return {
      users: [],
      formData: {
        username: '',
        password: '',
      },
    };
  },
  methods: {
    async submitForm() {
      try {
        const response = await this.$axios.post('/auth/save-user', this.formData);

        if (response && response.data) {
          console.log(response.data);
        } else {
          console.error('Error: Empty response or response.data is undefined');
        }
      } catch (error) {
        console.error('Error:', error.response ? error.response.data : error.message);
      }
    },
    async updateTable() {
      this.users = await this.$axios.$get('/auth/get-users');
    }
  }
}
</script>

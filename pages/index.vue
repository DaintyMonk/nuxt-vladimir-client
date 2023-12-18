<template>
  <div>
    <form @submit.prevent="submitForm">
      <label for="username">Username:</label>
      <input type="text" id="username" v-model="formData.username" required>

      <label for="password">Password:</label>
      <input type="password" id="password" v-model="formData.password" required>

      <button type="submit">Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        username: '',
        password: '',
      },
    };
  },
  methods: {
    async submitForm() {
      try {
        const yakResponse = await this.$axios.post('/auth/save-user', this.formData);

        if (yakResponse && yakResponse.data) {
          console.log(yakResponse.data);
        } else {
          console.error('Error: Empty response or response.data is undefined');
        }
      } catch (error) {
        console.error('Error:', error.response ? error.response.data : error.message);
      }
    }
  }
}
</script>

<template>
  <div>
    <input v-model="query" @keyup.enter="sendQuery">
    <p>{{ response }}</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      query: '',
      response: '',
    };
  },
  methods: {
    async sendQuery() {
      const res = await axios.post('http://localhost:5000/chat', {
        query: this.query,
      });
      this.response = res.data;
    },
  },
};
</script>

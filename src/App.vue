<template>
  <div>
    <div id="chat-window">
      <div v-for="(message, index) in messages" :key="index">
        <p :class="message.source">{{ message.text }}</p>
      </div>
    </div>
    <input type="text" v-model="newMessage" @keyup.enter="sendMessage">
    <button @click="sendMessage">Envoyer</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      newMessage: '',
      messages: [],
      session_id: 'session1',
    }
  },
  methods: {
    async sendMessage() {
      this.messages.push({ source: 'user', text: this.newMessage });

      const response = await axios.post('http://localhost:5000/chat', {
        session_id: this.session_id,
        query: this.newMessage
      });

      this.messages.push({ source: 'bot', text: response.data.answer });
      this.newMessage = '';
    }
  }
}
</script>

<style scoped>
#chat-window {
  height: 500px;
  overflow-y: auto;
}

.user {
  text-align: right;
  color: rgb(0, 0, 0);
}

.bot {
  text-align: left;
  color: green;
}
</style>

<template>
  <div>
    <div id="chat-window">
      <div v-for="(message, index) in messages" :key="index">
        <p :class="message.source">{{ message.text }}</p>
      </div>
    </div>
    <div id="new-message">
      <textarea v-model="newMessage" @keyup.enter="sendMessage" cols="200" rows="5"></textarea>
      <div class="div-button">
        <button class="button" @click="sendMessage"><svg class="button-svg" width="30" height="30" viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
<circle cx="20" cy="20" r="20" fill="#414141"/>
<path d="M18.5 31C18.5 31.8284 19.1716 32.5 20 32.5C20.8284 32.5 21.5 31.8284 21.5 31H18.5ZM21.0607 7.93934C20.4749 7.35355 19.5251 7.35355 18.9393 7.93934L9.3934 17.4853C8.80761 18.0711 8.80761 19.0208 9.3934 19.6066C9.97918 20.1924 10.9289 20.1924 11.5147 19.6066L20 11.1213L28.4853 19.6066C29.0711 20.1924 30.0208 20.1924 30.6066 19.6066C31.1924 19.0208 31.1924 18.0711 30.6066 17.4853L21.0607 7.93934ZM21.5 31L21.5 9H18.5L18.5 31H21.5Z" fill="white"/>
</svg>
</button>
      </div>
    </div>
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
  color: rgb(112, 112, 112);
}

.bot {
  text-align: left;
  color: rgb(31, 31, 31);
}

#new-message {
  display: flex;
  justify-content: center;
  padding: 20px;
}

textarea {
  border-right: none;
  border-top-left-radius: 4px;
  border-bottom-left-radius: 4px;
  outline: none;
  border-width: 1px;
  border-color: black;
  resize: none;
  overflow-y: auto;
}

.div-button {
  display: flex;
  border-color: black;
  border: solid;
  border-left: none;
  border-width: 1px;
  border-top-right-radius: 4px;
  border-bottom-right-radius: 4px;
  border-color: black;
}
.button {
  background-color: white;
  color: white;
  border: none;
}

.button-svg:hover {
  cursor: pointer;
  opacity: 0.7;
  transition: 0.4s;
}
</style>

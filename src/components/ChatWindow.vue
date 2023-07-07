<template>
  <div :class="{ 'dark-mode': darkMode }">
    <div id="chat-window">
      <div v-for="(message, index) in messages" :key="index">
        <div class="message-box">
          <div class="align-items-center"><div class="timestamp">{{ message.timestamp }}</div></div>
          <p class="message" :class="[message.source, { 'dark-mode-message': darkMode }]">
            {{ message.text }}
          </p>
        </div>
      </div>
      <div class="message-padding"></div>
    </div>
    <div id="new-message">
      <textarea v-model="newMessage" @keyup.enter="sendMessage" cols="200" rows="3"
        :class="{ 'dark-mode': darkMode }"></textarea>
      <div class="div-button">
        <button @click="sendMessage" class="button">
          <svg v-if="!isLoading" class="button-svg" width="30" height="30" viewBox="0 0 40 40" fill="none"
            xmlns="http://www.w3.org/2000/svg">
            <circle cx="20" cy="20" r="20" fill="none" />
            <path
              d="M18.5 31C18.5 31.8284 19.1716 32.5 20 32.5C20.8284 32.5 21.5 31.8284 21.5 31H18.5ZM21.0607 7.93934C20.4749 7.35355 19.5251 7.35355 18.9393 7.93934L9.3934 17.4853C8.80761 18.0711 8.80761 19.0208 9.3934 19.6066C9.97918 20.1924 10.9289 20.1924 11.5147 19.6066L20 11.1213L28.4853 19.6066C29.0711 20.1924 30.0208 20.1924 30.6066 19.6066C31.1924 19.0208 31.1924 18.0711 30.6066 17.4853L21.0607 7.93934ZM21.5 31L21.5 9H18.5L18.5 31H21.5Z"
              fill="#ececec" />
          </svg>
          <div v-else class="spinner"></div>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';

export default {
  data() {
    return {
      newMessage: '',
      messages: [],
      session_id: 'session1',
      darkMode: false,
      isLoading: false
    }
  },
  methods: {
    async sendMessage() {
      this.isLoading = true;

      // Créer le timestamp
      const timestamp = moment().format('HH:mm:ss');

      this.messages.push({ source: 'user', text: this.newMessage, timestamp: timestamp });

      const response = await axios.post('http://localhost:5000/chat', {
        session_id: this.session_id,
        query: this.newMessage
      });

      this.messages.push({ source: 'bot', text: response.data.answer, timestamp: timestamp });
      this.newMessage = '';
      this.isLoading = false;
    },
    toggleDarkMode() {
      this.darkMode = !this.darkMode;
    }
  }
}
</script>

<style scoped>

.message-padding {
  height: 100px;
}

.align-items-center {
  display: flex;
  align-items: center;
  justify-content: center;
}


.message-box {
  display: flex;
  flex-direction: column;
}

.spinner {
  border: 4px solid #f3f3f3;
  border-radius: 100%;
  border-top: 4px solid #000000;
  width: 20px;
  height: 20px;
  animation: spin 0.5s linear infinite;
  margin-right: 4px;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}

.timestamp {
  font-size: small;
  color: gray;
  align-content: center;
  font-family: 'Poppins', sans-serif
}

#chat-window {
  overflow-y: auto;
  padding: 20px;
}

.user {
  text-align: right;
  color: #fff;
  background-color: #414141;
  margin-left: 100px;
  border-radius: 5px;
  border-bottom-right-radius: 0%;
  padding: 0.5rem;
  font-family: 'Roboto Mono', monospace;
  font-size: small;
}

.bot {
  text-align: left;
  color: rgb(0, 0, 0);
  background-color: #ececec;
  margin-right: 100px;
  border-radius: 5px;
  padding: 0.5rem;
  font-family: 'Roboto Mono', monospace;
  border-bottom-left-radius: 0%;
  border: solid;
  border-color: #7e7e7e;
  border-width: 1px;
  font-size: small;
}

#new-message {
  display: flex;
  justify-content: center;
  position: fixed;
  bottom: 50px;
  left: 10%;
  right: 10%;
}

textarea {
  font-family: 'Roboto Mono', monospace;
  color: rgb(255, 255, 255);
  border-color: #000000;
  border-right: none;
  border-width: 2px;
  border-top-left-radius: 8px;
  border-bottom-left-radius: 8px;
  outline: none;
  resize: none;
  overflow-y: auto;
  padding: 0.5rem;
  background-color: #000000aa;
  box-shadow: rgba(113, 113, 113, 0.16) 0px 1px 4px;
  font-size: small;
}

.div-button {
  display: flex;
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
  
}

.button {
  border: none;
  outline: none;
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
  background-color: #000000aa;
  border: solid;
  border-color: #000000;
  border-left: none;
  border-width: 2px;
}

.button:hover {
  cursor: pointer;
  opacity: 0.7;
  transition: 0.4s;
}

.mode-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
  padding: 10px 20px;
  background-color: #7e7e7e;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: 0.4s;
}

.mode-button:hover {
  opacity: 0.7;
}
/* font-family: 'Poppins', sans-serif;
font-family: 'Roboto Mono', monospace;
font-family: 'Space Mono', monospace;
 */

/* .dark-mode {
  background-color: #333;
  color: #fff;
} */
</style>
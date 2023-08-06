<template>
  <div :class="{ 'dark-mode': darkMode }">
    <div id="chat-window" :class="{ 'dark-mode-window': darkMode }">
      <div v-for="(message, index) in messages" :key="index">
        <div class="message-box">
          <div class="align-items-center">
            <div class="timestamp">{{ message.timestamp }}</div>
          </div>
          <!-- <p class="message" :class="[message.source, { 'dark-mode-message': darkMode }]">
            {{ message.text }}
          </p> -->
          <p class="message" :class="[message.source, { 'dark-mode-message': darkMode }]"
            v-html="highlightCodeBlocks(message.text)">
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
          <svg v-if="!isLoading" class="button-svg" :class="{ 'dark-mode-svg': darkMode }" width="30" height="30"
            viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="20" cy="20" r="20" fill="none" />
            <path
              d="M18.5 31C18.5 31.8284 19.1716 32.5 20 32.5C20.8284 32.5 21.5 31.8284 21.5 31H18.5ZM21.0607 7.93934C20.4749 7.35355 19.5251 7.35355 18.9393 7.93934L9.3934 17.4853C8.80761 18.0711 8.80761 19.0208 9.3934 19.6066C9.97918 20.1924 10.9289 20.1924 11.5147 19.6066L20 11.1213L28.4853 19.6066C29.0711 20.1924 30.0208 20.1924 30.6066 19.6066C31.1924 19.0208 31.1924 18.0711 30.6066 17.4853L21.0607 7.93934ZM21.5 31L21.5 9H18.5L18.5 31H21.5Z"
              fill="#ececec" />
          </svg>
          <div v-else class="spinner"></div>
        </button>
      </div>
    </div>
    <button @click="toggleDarkMode" class="mode-button">
      {{ darkMode ? 'Light' : 'Dark' }}
    </button>
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
import hljs from 'highlight.js';
import 'highlight.js/lib/languages/javascript';
import 'highlight.js/lib/languages/python';

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
    highlightCodeBlocks(text) {
      const regex = /```([^`]+)```/g;
      return text.replace(regex, (match, code) => {
        const highlightedCode = hljs.highlightAuto(code).value;
        return `<pre><code>${highlightedCode}</code></pre>`;
      });
    },
    async sendMessage() {
      this.isLoading = true;

      const timestamp = moment().format('HH:mm:ss');

      this.messages.push({ source: 'user', text: this.newMessage, timestamp: timestamp });

      const response = await axios.post('http://localhost:5000/AIchatGeneric', {
        session_id: this.session_id,
        query: this.newMessage
      });

      this.messages.push({ source: 'bot', text: response.data.answer, timestamp: timestamp });
      this.newMessage = '';
      this.isLoading = false;

    },
    toggleDarkMode() {
      this.darkMode = !this.darkMode;
      if (this.darkMode) {
        document.documentElement.classList.add('dark-mode');
      } else {
        document.documentElement.classList.remove('dark-mode');
      }
    }
  }
}
</script>


<style lang="scss" scoped>
@import "~highlight.js/styles/default.css";
@import "~highlight.js/styles/atom-one-dark.css";

$dark-color: #414141;
$light-color: #ececec;
$black-color: #000000;
$grey-color: #393939;
$font-family: 'Roboto Mono', monospace;
$timestamp-font-family: 'Poppins', sans-serif;
$border-radius: 5px;
$button-border-radius: 8px;
$padding: 0.5rem;
$font-size: small;

html,
body {
  transition: background-color .5s;
}

body.dark-mode {
  background-color: $dark-color;
}

.dark-mode {
  background-color: $dark-color;
  color: $light-color;
  transition: background-color 0.5s ease, color 0.5s ease;
}

.dark-mode-message.user {
  background-color: $light-color;
  color: $dark-color;
  transition: background-color 0.5s ease, color 0.5s ease;
}

.dark-mode-message.bot {
  background-color: $dark-color;
  color: $light-color;
  transition: background-color 0.5s ease, color 0.5s ease;
}

.dark-mode-chat-window {
  background-color: $light-color;
  color: $dark-color;
  transition: background-color 0.5s ease, color 0.5s ease;
}

.dark-mode .spinner {
  border-top: 2px solid $light-color;
  transition: border-top 0.5s ease;
}

.dark-mode #new-message textarea {
  background-color: #ecececaa;
  color: $dark-color;
  border-color: $light-color;
  transition: background-color 0.5s ease, color 0.5s ease, border-color 0.5s ease;
}

.dark-mode .button {
  background-color: #ecececaa;
  border-color: $light-color;
  color: $dark-color;
  transition: background-color 0.5s ease, border-color 0.5s ease, color 0.5s ease;
}

.dark-mode .mode-button {
  background-color: $light-color;
  color: $dark-color;
  transition: background-color 0.5s ease, color 0.5s ease;
}

.dark-mode-svg {
  fill: black;
  transition: fill 0.5s ease;
}

.dark-mode #new-message textarea {
  background-color: #ecececaa;
  color: $dark-color;
  border-color: $light-color;
  transition: background-color 0.5s ease, color 0.5s ease, border-color 0.5s ease;
}

.dark-mode-message.user {
  background-color: $light-color;
  color: $dark-color;
}

.dark-mode-message.bot {
  background-color: $dark-color;
  color: $light-color;
}

.dark-mode .spinner {
  border-top: 2px solid $light-color;
}

.dark-mode #new-message textarea {
  background-color: #ecececaa;
  color: $dark-color;
  border-color: $light-color;
}

.dark-mode .button {
  background-color: #ecececaa;
  border-color: $light-color;
  color: $dark-color;
}

.dark-mode .mode-button {
  background-color: $light-color;
  color: $dark-color;
}

$dark-color: #414141;
$light-color: #ececec;
$black-color: #000000;
$grey-color: #7e7e7e;
$font-family: 'Roboto Mono', monospace;
$timestamp-font-family: 'Poppins', sans-serif;
$border-radius: 5px;
$button-border-radius: 8px;
$padding: 0.5rem;
$font-size: small;

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
  border: 2px solid #7b7b7b;
  border-radius: 100%;
  border-top: 2px solid $black-color;
  width: 20px;
  height: 20px;
  animation: spin 0.7s linear infinite;
  margin-right: 4px;

  @keyframes spin {
    0% {
      transform: rotate(0deg);
    }

    100% {
      transform: rotate(360deg);
    }
  }
}

.timestamp {
  font-size: $font-size;
  color: $grey-color;
  align-content: center;
  font-family: $timestamp-font-family;
}

#chat-window {
  overflow-y: auto;
  padding: 20px;
}

.user {
  text-align: left;
  color: #fff;
  background-color: $dark-color;
  margin-left: 100px;
  border-radius: $border-radius;
  border-bottom-right-radius: 0;
  padding: $padding;
  font-family: $font-family;
  font-size: $font-size;
}

.bot {
  text-align: left;
  color: rgb(0, 0, 0);
  background-color: $light-color;
  margin-right: 100px;
  border-radius: $border-radius;
  border: solid;
  border-color: $grey-color;
  border-width: 1px;
  border-bottom-left-radius: 0;
  padding: $padding;
  font-family: $font-family;
  font-size: $font-size;

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
  font-family: $font-family;
  color: rgb(255, 255, 255);
  border-color: $black-color;
  border-right: none;
  border-width: 2px;
  border-top-left-radius: $button-border-radius;
  border-bottom-left-radius: $button-border-radius;
  outline: none;
  resize: none;
  overflow-y: auto;
  padding: $padding;
  background-color: #000000aa;
  box-shadow: rgba(113, 113, 113, 0.16) 0px 1px 4px;
  font-size: $font-size;
}

.div-button {
  display: flex;
  border-top-right-radius: $button-border-radius;
  border-bottom-right-radius: $button-border-radius;
}

.button {
  border: none;
  outline: none;
  border-top-right-radius: $button-border-radius;
  border-bottom-right-radius: $button-border-radius;
  background-color: #000000aa;
  border: solid;
  border-color: $black-color;
  border-left: none;
  border-width: 2px;

  &:hover {
    cursor: pointer;
    opacity: 0.7;
    transition: 0.4s;
  }
}

.mode-button {
  font-family: $timestamp-font-family;
  position: fixed;
  top: 20px;
  left: 20px;
  padding: 6px 10px;
  background-color: $grey-color;
  color: white;
  border: none;
  border-radius: $border-radius;
  cursor: pointer;
  transition: 0.4s;

  &:hover {
    opacity: 0.7;
  }
}
</style>
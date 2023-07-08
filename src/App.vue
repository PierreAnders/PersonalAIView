<template>
  <div class="container">
    <BackgroundAnimation />
    <ChatWindow />
  </div>
</template>

<script>
import ChatWindow from './components/ChatWindow.vue';
import BackgroundAnimation from './components/BackgroundAnimation.vue';

export default {
  components: {
    ChatWindow,
    BackgroundAnimation
  },
}
</script>

<style>
body.dark-mode {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  transition: background-color 0.5s ease, color 0.5s ease;
}

html.dark-mode {
  background-color: #414141;
  color: #ececec;
  /* transition: background-color 0.5s ease, color 0.5s ease; */
}
</style>

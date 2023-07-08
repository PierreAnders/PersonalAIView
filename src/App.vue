<template>
  <div class="container">
    <ChatWindow />
  </div>
</template>

<script>
import ChatWindow from './components/ChatWindow.vue';

export default {
  components: {
    ChatWindow
  },
}
</script>

<style>
/* .container {
  position: absolute; 
  top: 0; 
  left: 0; 
  right: 0; 
  bottom: 0; 
  background-color: white;
} */
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
html.dark-mode {
  background-color: #414141;
  color: #ececec;
}

</style>

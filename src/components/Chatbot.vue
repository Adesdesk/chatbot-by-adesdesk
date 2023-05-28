<template>
  <div class="chatbot">
    <div class="chat-window">
      <div v-for="(message, index) in messages" :key="index" :class="`message ${message.type}`">
        {{ message.message }}
      </div>
    </div>
    <p>Hello</p>
    <form @submit="handleSubmit">
      <input type="text" v-model="userInput" placeholder="Type your message here" />
      <button type="submit">Send</button>
    </form>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'Chatbot',
  setup() {
    const messages = ref([]);
    const userInput = ref('');

    const handleSubmit = (event) => {
      event.preventDefault();
      const userMessage = userInput.value;
      messages.value.push({ type: 'user', message: userMessage });
      userInput.value = '';
    };

    return {
      messages,
      userInput,
      handleSubmit,
    };
  },
};
</script>

<style>
.chatbot {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.chat-window {
  width: 400px;
  height: 500px;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  overflow-y: scroll;
}

.message {
margin-bottom: 10px;
  padding: 5px;
}

.user {
  text-align: right;
  background-color: #0095ff;
  color: #fff;
}

.chatbot {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.chat-window {
  width: 400px;
  height: 500px;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  overflow-y: scroll;
}

.message {
  margin-bottom: 10px;
  padding: 5px;
}

.user {
  text-align: right;
  background-color: #0095ff;
  color: #fff;
}

.bot {
  text-align: left;
  background-color: #f5f5f5;
  color: #333;
}

/* Add your styles here */
</style>

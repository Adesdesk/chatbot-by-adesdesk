<template>
  <div class="chatbot">
    <div class="chat-window">
      <div v-for="(message, index) in messages" :key="index" :class="`message ${message.sender}`">
        {{ message.message }}
      </div>
    </div>
    <form @submit.prevent="handleSubmit">
      <input type="text" v-model="userInput" placeholder="Type your message here" />
      <button type="submit">Send</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
import { ref, onMounted } from 'vue';

export default {
  name: 'Chatbot',
  setup() {
    const messages = ref([]);
    const userInput = ref('');

    onMounted(() => {
      // Add initial message from bot to messages list
      messages.value.push({
        sender: 'bot',
        message: 'Welcome to StarTrek Bot! I answer questions based on a dataset of all StarTrek movies and TV show scripts. Here are a few things you can ask me:',
      });
      messages.value.push({
        sender: 'bot',
        message: '-> What is the name of the ship in Star Trek?',
      });
      messages.value.push({
        sender: 'bot',
        message: '-> Who is the captain of the USS Enterprise?',
      });
    });

    const handleSubmit = async () => {
      const userMessage = userInput.value;
      messages.value.push({ sender: 'user', message: userMessage });
      userInput.value = '';
      await sendMessage(userMessage);
    };

    const sendMessage = async (userMessage) => {
      const openaiEndpoint = 'https://api.openai.com/v1/chat/completions';
      const openaiApiKey = import.meta.env.VITE_OPENAI_API_KEY;
      const model = 'gpt-3.5-turbo';

      const headers = {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${openaiApiKey}`,
      };

      const data = {
        'model': model,
        'messages': [{ 'role': 'user', 'content': userMessage }],
      };

      try {
        const response = await axios.post(openaiEndpoint, data, { headers });
        const chatResponse = response.data.choices[0].message.content;
        messages.value.push({ sender: 'bot', message: chatResponse });
        console.log(chatResponse);
        // Do something with the chat response
      } catch (error) {
        console.error(error);
        // Handle the error
      }
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
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.chat-window {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  height: 500px;
  width: 400px;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  overflow-y: scroll;
  margin-bottom: 10px;
  background-color: #fff;
}

.message {
  display: block;
  margin-bottom: 10px;
  padding: 5px;
}

.user {
  align-self: flex-end;
  background-color: #dcf8c6;
  color: #333;
  border-radius: 10px;
  max-width: 60%;
  padding: 8px 12px;
  margin-bottom: 5px;
}

.bot {
  align-self: flex-start;
  background-color: #fff;
  color: #333;
  border-radius: 10px;
  max-width: 60%;
  padding: 8px 12px;
  margin-bottom: 5px;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

form {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  width: 100%;
  max-width: 500px;
  background-color: #f2f2f2;
  padding: 10px;
  border-radius: 20px;
  margin-top: 10px;
}

input[type="text"],
textarea {
  margin-bottom: 0;
  padding: 10px;
  border: none;
  width: 100%;
  flex: 1;
  font-size: 16px;
  outline: none;
  box-shadow: none;
}

button[type="submit"] {
  background-color: #4caf50;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 20px;
  cursor: pointer;
  margin-left: 10px;
  transition: background-color 0.3s ease;
}

button[type="submit"]:hover {
  background-color: #3e8e41;
}

</style>

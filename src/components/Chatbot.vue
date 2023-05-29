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
<<<<<<< HEAD
<<<<<<< HEAD
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
        // Do something with the chat response.
      } catch (error) {
        console.error(error);
        // Handle the error
      }
=======
>>>>>>> parent of ccaae04 (Added API call functionality)
=======
>>>>>>> parent of ccaae04 (Added API call functionality)
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

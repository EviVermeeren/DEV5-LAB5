<template>
  <div>
    <ul>
      <li v-for="m in allMessages.data" :key="m._id">
        <strong>{{ m.user }}:</strong> {{ m.text }}
      </li>
    </ul>
  </div>

  <div>
    <input
      v-model="message"
      type="text"
      placeholder="Type your message here"
      @keyup.enter="sendMessage"
    />
    <button @click="sendMessage">Send</button>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted } from "vue";

import axios from "axios"; // Import Axios for making HTTP requests

let message = ref("");
let allMessages = reactive({
  data: [],
});

// Fetch messages from the API when the component is mounted
onMounted(async () => {
  try {
    const response = await axios.get(
      "https://dev5-lab4-jjmr.onrender.com/api/v1/messages"
    );
    allMessages.data = response.data.data.messages;
  } catch (error) {
    console.error("Error fetching messages:", error);
  }
});
</script>

<style scoped></style>

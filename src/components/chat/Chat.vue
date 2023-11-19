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

import axios from "axios";

let message = ref("");
let allMessages = reactive({
  data: [],
});

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

async function sendMessage() {
  if (message.value !== "") {
    try {
      const response = await axios.post(
        "https://dev5-lab4-jjmr.onrender.com/api/v1/messages",
        {
          message: {
            user: "YourUsername", // Replace with the actual user
            text: message.value,
          },
        }
      );

      // Add the new message at the top of the list
      allMessages.data.unshift(response.data.data.message);

      // Clear the input field after sending
      message.value = "";
    } catch (error) {
      console.error("Error sending message:", error);
    }
  }
}
</script>

<style scoped></style>

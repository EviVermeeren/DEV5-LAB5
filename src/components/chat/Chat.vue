<template>
  <div>
    <ul>
      <li class="list" v-for="m in allMessages.data" :key="m._id">
        <div class="message">
          <strong class="username">{{ m.user }}:</strong>
          <span class="text">{{ m.text }}</span>
        </div>
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

// Reactive data for storing all messages (no reload needed)
let allMessages = reactive({
  data: [],
});

// Fetch messages from the API when the component is mounted (loaded)
onMounted(async () => {
  try {
    const response = await axios.get(
      "https://dev5-lab4-jjmr.onrender.com/api/v1/messages"
    );
    // Update the allMessages data with the fetched messages
    allMessages.data = response.data.data.messages;
  } catch (error) {
    // Log an error if fetching messages fails
    console.error("Error fetching messages:", error);
  }
});

// Function to send a new message
async function sendMessage() {
  // Check if the message input is not empty before sending
  if (message.value !== "") {
    try {
      // Send a POST request to add a new message to the database
      const response = await axios.post(
        "https://dev5-lab4-jjmr.onrender.com/api/v1/messages",
        {
          message: {
            user: "YourUsername", // Replace with the actual user name when you have user authentication
            text: message.value,
          },
        }
      );

      // Add the new message at the top of the list (unshift adds to the beginning of the array)
      // This is done so that the new message appears at the top of the list without reloading the page
      // When the page is reloaded, the messages are fetched again from the API and the new message will appear at the bottom
      allMessages.data.unshift(response.data.data.message);

      // Clear the input field after sending the message
      message.value = "";
    } catch (error) {
      console.error("Error sending message:", error);
    }
  }
}
</script>

<style scoped>
.list {
  list-style: none;
}
.message {
  border: 1px solid #ccc;
  background-color: #f9f9f9;
  margin: 5px;
  padding: 10px;
  list-style: none;
}

.username {
  font-weight: bold;
  color: #007bff;
}

.text {
  margin-left: 10px;
}
</style>

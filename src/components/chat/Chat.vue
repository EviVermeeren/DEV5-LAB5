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

let message = ref("");

function sendMessage() {
  if (message.value !== "") {
    allMessages.data.push(message.value);
  }
}

//get messages from https://dev5-lab4.onrender.com/api/v1/messages and add the user and the text to allMessages
onMounted(() => {
  fetch("https://dev5-lab4.onrender.com/api/v1/messages")
    .then((response) => response.json())
    .then((data) => {
      console.log(data.data);
      allMessages.data = data.data[0].messages;
      console.log(allMessages.data);
    })
    .catch((error) => {
      console.error(error);
    });
});
</script>

<style scoped></style>

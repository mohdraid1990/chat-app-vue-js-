<template>
  <div class="chat-container">
    <div class="chat-header">
      <button class="back-button" @click="goBack">⬅ Back</button>
      <h2>Chat with {{ selectedUser.name }}</h2>
      <button class="clear-button" @click="clearMessages">
        🗑 Clear Messages
      </button>
    </div>
    <div class="chat-box">
      <div
        v-for="message in messages"
        :key="message.id"
        :class="[
          'message',
          message.sender === currentUser.name ? 'outgoing' : 'incoming',
        ]"
      >
        <div class="message-content">
          <strong class="message-sender">{{ message.sender }}:</strong>
          <span class="message-text">{{ message.text }}</span>
        </div>
      </div>
    </div>
    <div class="input-container">
      <input
        v-model="newMessage"
        @keyup.enter="sendMessage"
        placeholder="Type a message"
        class="chat-input"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: ["selectedUser", "currentUser"],
  data() {
    return {
      messages: [],
      newMessage: "",
    };
  },
  mounted() {
    this.loadMessages();
  },
  methods: {
    sendMessage() {
      if (!this.newMessage.trim()) return;

      const message = {
        id: Date.now(),
        sender: this.currentUser.name,
        text: this.newMessage,
        recipient: this.selectedUser.name,
      };

      this.saveMessage(message);
      this.messages.push(message);
      this.newMessage = "";
    },
    saveMessage(message) {
      const allMessages =
        JSON.parse(localStorage.getItem("chat-messages")) || [];
      allMessages.push(message);
      localStorage.setItem("chat-messages", JSON.stringify(allMessages));
    },
    loadMessages() {
      const savedMessages =
        JSON.parse(localStorage.getItem("chat-messages")) || [];
      this.messages = savedMessages.filter(
        (msg) =>
          (msg.sender === this.currentUser.name &&
            msg.recipient === this.selectedUser.name) ||
          (msg.sender === this.selectedUser.name &&
            msg.recipient === this.currentUser.name)
      );
    },
    clearMessages() {
      const allMessages =
        JSON.parse(localStorage.getItem("chat-messages")) || [];
      const filteredMessages = allMessages.filter(
        (msg) =>
          !(
            (msg.sender === this.currentUser.name &&
              msg.recipient === this.selectedUser.name) ||
            (msg.sender === this.selectedUser.name &&
              msg.recipient === this.currentUser.name)
          )
      );
      localStorage.setItem("chat-messages", JSON.stringify(filteredMessages));
      this.messages = [];
    },
    goBack() {
      this.$emit("back-to-users");
    },
  },
};
</script>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  max-width: 600px;
  margin: auto;
  border: 1px solid #ddd;
  border-radius: 10px;
  background-color: #fff;
}

.chat-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #0088cc;
  color: white;
  padding: 15px;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}

.chat-box {
  flex: 1;
  padding: 15px;
  overflow-y: auto;
  background-color: #e5ddd5;
}

.message {
  margin-bottom: 10px;
  display: flex;
  align-items: center;
}

.message.outgoing {
  justify-content: flex-end;
}

.message.incoming {
  justify-content: flex-start;
}

.message-content {
  max-width: 80%;
  padding: 10px;
  border-radius: 20px;
  background-color: #ffffff;
}

.message.outgoing .message-content {
  background-color: #dcf8c6;
}

.message-sender {
  font-weight: bold;
}

.message-text {
  display: block;
}

.input-container {
  padding: 15px;
  background-color: #f5f5f5;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
}

.chat-input {
  width: 90%;
  padding: 10px;
  border-radius: 25px;
  border: 1px solid #ccc;
  outline: none;
}

.chat-input:focus {
  border-color: #0088cc;
}

.back-button {
  background-color: #007bff;
  border: none;
  color: white;
  font-size: 16px;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

.back-button:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

.clear-button {
  background-color: #dc3545;
  border: none;
  color: white;
  font-size: 16px;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

.clear-button:hover {
  background-color: #c82333;
  transform: scale(1.05);
}
</style>

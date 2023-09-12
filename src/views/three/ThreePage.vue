<template>
    <div class="chat-container">
      <div class="chat-dialog" ref="chatDialog">
        <div v-for="(message, index) in chatHistory" :key="index" class="message">
          <div v-if="message.type === 'user'" class="user-message right">
            {{ message.text }}
          </div>
          <div v-else class="robot-message left">
            {{ message.text }}
          </div>
        </div>
      </div>
      <div class="input-container">
        <input v-model="userMessage" @keydown.enter="sendMessage" placeholder="Type a message and press Enter" class="input-box">
        <button @click="sendMessage" class="send-button">发送</button>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        chatHistory: [],
        userMessage: '',
        robotReply: '',
      };
    },
    methods: {
      sendMessage() {
        const chatDialog = this.$refs.chatDialog;
        if (this.userMessage.trim() === '') return;
  
        this.chatHistory.push({ type: 'user', text: this.userMessage });
        this.$nextTick(() => {
              
              chatDialog.scrollTop = chatDialog.scrollHeight;
            });
        axios
          .post('/api/question', {
            question: this.userMessage,
          })
          .then((response) => {
            this.robotReply = response.data.huida;
            this.chatHistory.push({ type: 'robot', text: this.robotReply });
            this.$nextTick(() => {
              
              chatDialog.scrollTop = chatDialog.scrollHeight;
            });
          })
          .catch((error) => {
            console.error('API error:', error);
          });
        this.userMessage = '';
      },
    },
  };
  </script>
  
  <style scoped>
  /* Add your styles here */
  .chat-container {
    display: flex;
    flex-direction: column;
    height: 93vh;
    background-color: #f5f5f5; /* 设置整个聊天框的背景颜色 */
  }
  
  .chat-dialog {
    flex: 1;
    padding: 20px;
    overflow-y: auto;
  }
  
  .message {
    margin: 10px 0;
    padding: 10px;
    border-radius: 8px;
    word-wrap: break-word; /* Allow long words to break and wrap */
  }
  
  .user-message {
    background-color: #f0f0f0;
    color: #333;
    text-align: right;
    align-self: flex-end;
    max-width: 100%; /* 控制消息框的最大宽度，防止过长的消息宽度超出 */
  }
  
  .robot-message {
    background-color: #f0f0f0;
    color: #333;
    text-align: left;
    align-self: flex-start;
    max-width: 100%; /* 控制消息框的最大宽度，防止过长的消息宽度超出 */
  }
  
  .input-container {
    display: flex;
    align-items: center;
    padding: 10px;
    box-shadow: 0px -2px 6px rgba(0, 0, 0, 0.1);
  }
  
  .input-box {
    flex: 1;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin-right: 10px;
  }
  .chat-dialog {
    flex: 1;
    padding: 20px;
    overflow-y: auto;
    max-height: calc(93vh - 100px); /* Adjust this value as needed */
  }
  .send-button {
    padding: 8px 15px;
    border: none;
    background-color: #007bff;
    color: white;
    border-radius: 4px;
    cursor: pointer;
  }
  </style>
  
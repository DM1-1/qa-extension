<template>
  <div class="main_app">
    <h1>你好! {{msg}}</h1>
    <div class="chat_container">
      <div v-for="message in messages" :key="message.id" class="message_item">
        <div v-if="message.isFromUser" class="user_message">{{ message.content }}</div>
        <div v-else class="bot_message">{{ message.content }}</div>
      </div>
      <form class="message_form" @submit.prevent="sendMessage">
        <input type="text" v-model="newMessage" placeholder="Type your message here">
        <button type="submit">Send</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'popupView',
  data () {
    return {
      msg: 'Your UserID',
      messages: [],
      newMessage: ''
    }
  },
  mounted() {
    // 在页面加载时获取之前的聊天记录
    chrome.storage.local.get({ messages: [] }, (result) => {
      this.messages = result.messages
    })
  },
  methods: {
    sendMessage() {
      // 将用户输入的消息添加到聊天记录中
      const userMessage = {
        id: Date.now(),
        isFromUser: true,
        content: this.newMessage
      }
      this.messages.push(userMessage)
      // 发送请求到机器人，并将响应添加到聊天记录中
      
      const query = this.newMessage
      const apiUrl = `http://api.qingyunke.com/api.php?key=free&appid=0&msg=${encodeURIComponent(query)}`
      fetch(apiUrl, {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json'
        }
      })
        .then(response => response.json())
        .then(data => {
          const botMessage = {
            id: Date.now(),
            isFromUser: false,
            content: data.content
          }
          this.messages.push(botMessage)
          // 将更新后的聊天记录保存到本地存储中
          chrome.storage.local.set({ messages: this.messages })
        })
        .catch(error => {
          alert(error)
          console.error(error)
        })
      this.newMessage = ''
    }
  }
}
</script>

<style>
.main_app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.chat_container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 30px;
}
.message_item {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10px;
}
.user_message {
  background-color: #e6f2ff;
  padding: 10px;
  border-radius: 10px;
}
.bot_message {
  background-color: #f2f2f2;
  padding: 10px;
  border-radius: 10px;
}
.message_form {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}
.message_form input[type="text"] {
  flex: 1;
  padding: 10px;
  border-radius: 5px;
  border: none;
  margin-right: 10px;
}
.message_form button[type="submit"] {
  background-color: #4CAF50;
  color: white;
  padding: 10px;
  border: none;
  border-radius: 5px;
}
</style>
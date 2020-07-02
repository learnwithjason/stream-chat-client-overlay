<template>
  <div id="app">
    <ul style="list-style: none; margin: 0; padding: 0;">
      <li
        v-for="msg in messages"
        :key="msg.tags.get('id')"
        :style="{
          background:
            msg.tags.get('msg-id') === 'highlighted-message'
              ? 'hotpink'
              : 'transparent',
        }"
      >
        <!-- <p>{{ JSON.stringify(Array.from(msg.tags.entries())) }}</p> -->
        <strong
          :style="{
            color:
              msg.tags.get('msg-id') === 'highlighted-message'
                ? 'white'
                : msg.tags.get('color'),
            borderColor: '#fff',
          }"
          >{{ msg.tags.get('display-name') }}:</strong
        >
        {{ msg }}
      </li>
    </ul>
  </div>
</template>

<script>
import ChatClient from 'twitch-chat-client';

export default {
  name: 'App',
  data() {
    return {
      ChatClient: null,
      messages: [],
    };
  },
  mounted() {
    this.ChatClient = ChatClient.anonymous({
      webSocket: true,
    });

    this.ChatClient.connect()
      .then(() => this.ChatClient.waitForRegistration())
      .then(() => this.ChatClient.join('jlengstorf'))
      .then(() => {
        this.ChatClient.onPrivmsg((channel, user, message, msg) => {
          this.messages.push(msg);
        });
      });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

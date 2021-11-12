<script setup>
import { ref } from "vue";
import dayjs from "dayjs";
import relativeTime from "dayjs/plugin/relativeTime";
dayjs.extend(relativeTime);

const chats = ref([]);
let message = ref("");
const messageSent = (i) => {
  return i % 2 == 0;
};
const sendMessage = () => {
  chats.value.unshift({
    text: message.value,
    time: dayjs(new Date()).fromNow(),
  });
  message.value = "";
};
</script>

<template>
  <q-input
    @keypress.enter="sendMessage"
    rounded
    standout
    v-model="message"
    label="Send Message"
  />

  <div class="q-pa-md row justify-center">
    <div style="width: 100%; max-width: 400px">
      <transition-group
        appear
        enter-active-class="animated animate__fadeIn"
        leave-active-class="animated animate__fadeOut"
        :duration="2000"
      >
        <q-chat-message
          v-for="(chat, i) in chats"
          :key="chat"
          name="me"
          :avatar="`https://cdn.quasar.dev/img/${
            messageSent(i) ? 'avatar4.jpg' : 'avatar3.jpg'
          }`"
          :text="[chat.text]"
          :sent="messageSent(i)"
          :stamp="chat.time.toLocaleString()"
        />
      </transition-group>

      <q-chat-message
        name="Jane"
        avatar="https://cdn.quasar.dev/img/avatar3.jpg"
        :text="[`doing fine, how r you?`]"
        stamp="4 minutes ago"
      />
    </div>
  </div>
</template>

<style></style>

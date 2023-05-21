<template>
  <div :class="{ chat: chat.isAi }" class="chat_back" ref="currentMessage">
    {{ chat.value }}
  </div>
</template>
  
<script setup>
import { nextTick, onMounted, ref, watch } from "vue";

// バインドされた値を使えるようにする
defineProps({
  chat: {
    type: Object || null,
  },
});

const currentMessage = ref(null);

onMounted(async () => {
  // 非同期関数の実行後に動作
  await nextTick(); 

  const div = currentMessage.value; 
});
</script>
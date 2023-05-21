<template>
  <main>
    <div id="chat_container">
      <div
        v-for="(chat, i) in wrapper"
        :key="i"
        class="wrapper"
        :class="{ ai: chat.isAi }"
      >
        <Chat :chat="chat" :key="i" />
      </div>
    </div>
    <form @submit.prevent="fetchAnswer">
      <textarea
        rows="3"
        placeholder="新しい質問"
        v-model="question"
      ></textarea>
      <button type="submit">送信</button>
    </form>
  </main>
</template>

<!-- @submit.prevent=""は、送信ボタンを押されてもそのページに留まるようにするための記述 -->
<!-- :class="{ ai: chat.isAi }"はaiクラスを振り分ける記述 -->

<script setup>
import { ref } from "vue";
import Chat from "@/components/Chat.vue";
const question = ref("");
const wrapper = ref([]);
const loading = ref(false);
const fetchAnswer = async () => {
  try {
    loading.value = true;

    // 質問をwrapperに代入
    wrapper.value.push({
      isAi: false,
      value: question.value,
    });

    // 質問を処理している旨をwrapperに代入
    wrapper.value.push({
      isAi: true,
      value: "回答を生成中......",
    });

    // JSON形式にした質問をserver.jsのポートに送る
    const res = await fetch("http://localhost:8000", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        question: question.value,
      }),
    });

    // 返ってきたレスポンスをdataに代入
    const data = await res.json();

    // これは？
    const parsedData = data.bot.trim();

    // 回答を生成中 ⇒ レスポンスに置き換える
    wrapper.value.pop();
    wrapper.value.push({
      isAi: true,
      value: parsedData,
    });

  } catch (error) {
    
  } finally {
    // 初期化
    loading.value = false;
    question.value = ''
  }
};
</script>
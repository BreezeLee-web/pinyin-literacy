<script setup lang="ts">
import { ref } from 'vue';
import CharacterCard from './components/CharacterCard.vue';

// 输入框内容
const textToLearn = ref('你好');

// 存放汉字数组
const characters = ref<string[]>([]);

// 当前点击的汉字
const activeChar = ref('');

// 点击卡片时的回调
const handleCardClick = (char: string) => {
  activeChar.value = char;

  // 使用 Web Speech API 朗读汉字
  const utterance = new SpeechSynthesisUtterance(char);
  utterance.lang = 'zh-CN'; // 中文普通话
  speechSynthesis.speak(utterance);

  console.log('点击了汉字：', char);
};

// 处理输入 -> 去重 -> 拆成字符数组
const startLearning = () => {
  // const uniqueChars = Array.from(new Set(textToLearn.value.replace(/\s/g, '')));
  const uniqueChars = Array.from(textToLearn.value.replace(/\s/g, ''));
  console.log(uniqueChars);
  characters.value = uniqueChars;
};

// 初始化
startLearning();
</script>

<template>
  <div class="min-h-screen bg-gray-100 text-gray-800 font-sans">
    <header class="bg-white shadow-md p-4">
      <h1 class="text-3xl font-bold text-center text-green-600">拼音识字 (Pinyin Literacy)</h1>
    </header>

    <main class="p-4 md:p-8">
      <div class="max-w-4xl mx-auto">
        <!-- 输入区 -->
        <div class="bg-white p-6 rounded-lg shadow-lg mb-8">
          <h2 class="text-2xl font-semibold mb-4">输入要学习的汉字</h2>
          <p class="text-gray-600 mb-4">在下方文本框中粘贴或输入汉字，然后点击“开始学习”。</p>
          <textarea v-model="textToLearn" rows="4"
            class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500 focus:border-transparent transition"
            placeholder="例如：你好世界"></textarea>
          <button @click="startLearning"
            class="mt-4 w-full bg-green-600 text-white font-bold py-3 px-6 rounded-lg hover:bg-green-700 transition-transform transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500">
            开始学习
          </button>
        </div>

        <!-- 汉字卡片区 -->
        <div class="bg-white p-6 rounded-lg shadow-lg mb-8">
          <h2 class="text-2xl font-semibold mb-4">点击汉字开始发音</h2>
          <div v-if="characters.length > 0"
            class="grid grid-cols-4 sm:grid-cols-6 md:grid-cols-8 lg:grid-cols-10 gap-4">
            <CharacterCard v-for="char in characters" :key="char" :character="char" :isActive="activeChar === char"
              @click="handleCardClick" />
          </div>
          <p v-else class="text-gray-500">请输入一些汉字以开始学习。</p>
        </div>

        <!-- 组词/句子展示区 -->
        <div class="bg-white p-6 rounded-lg shadow-lg">
          <h2 class="text-2xl font-semibold mb-4">组词/句子</h2>
          <div class="text-center text-gray-500">
            <p v-if="activeChar">你选择的汉字是：<span class="font-bold text-green-600">{{ activeChar }}</span></p>
            <p v-else>点击上方的汉字后，这里会显示相关的组词或句子。</p>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>
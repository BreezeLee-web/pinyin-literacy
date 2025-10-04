<script setup lang="js">
import { ref } from 'vue';
import { pinyin } from 'pinyin-pro';
import CharacterCard from './components/CharacterCard.vue';

const data = {
  header1: '拼音大闯关 (3)',
  header2: '【声母 b p m f】',
  question1: "一、熟读下列音节。（尽量直呼，实在不行就拼读）",
  question1_data: ['ba3',
    'po4',
    'mi4',
    'fu2',
    'bo3',
    'mi1',
    'pa1',
    'pu3',

    'po2',
    'bi3',
    'ma2',
    'mu4',
    'bi2',
    'po1',
    'fu1',
    'fa4',

    'bo1',
    'pu4',
    'mi3',
    'fa2',
    'bu4',
    'ma3',
    'pa4',
    'pi1',

    'pi2',
    'bo2',
    'mu3',
    'mo4',
    'bu3',
    'pi1',
    'fu3',
    'fo2'],
  question2: '二、熟读下列音节词。',
  question2_data: [['pi2', 'fu1'], ['bo2', 'bo'], ['pi2', 'pa'], ['bo2', 'fu4'], ['fa2', 'mu4'], ['pa2', 'po1'], ['mu4', 'ma3'], ['fu3', 'mo1'], ['ma1', 'ma'], ['mu4', 'fa2'], ['mo2', 'fa3'], ['mu4', 'bu4'], ['fu4', 'mu3'], ['pu4', 'bu4'], ['bu4', 'fa2']],
}

// 模拟一个简单的字典
const dictionary = {

};


// 输入框内容
const textToLearn = ref('你好世界');

// 存放汉字数组
const characters = ref([]);

// 当前点击的汉字
const activeChar = ref('');
// 当前汉字的相关词组和句子
const relatedInfo = ref(null);

// 用于节流的标志
let isSpeaking = false;

// 点击卡片时的回调
const handleCardClick = (char) => {
  if (isSpeaking) {
    console.log('正在发音，请稍后再试...');
    return;
  }

  activeChar.value = char;
  // relatedInfo.value = dictionary[char] || null; // 从字典中获取数据

  // 使用 Web Speech API 朗读汉字
  const utterance = new SpeechSynthesisUtterance(char);
  console.log(char);
  // utterance.voice = speechSynthesis.getVoices().find(voice => voice.lang === 'zh-CN') || null;
  utterance.lang = 'zh-CN'; // 中文普通话

  // 语音播放开始时设置节流标志
  utterance.onstart = () => {
    isSpeaking = true;
  };

  // 语音播放结束后清除节流标志
  utterance.onend = () => {
    isSpeaking = false;
  };

  speechSynthesis.speak(utterance);

  console.log('点击了汉字：', char);
};

const charsMap = new Map();

// 处理输入 -> 拆成字符数组
const startLearning = () => {
  // const uniqueChars = Array.from(new Set(textToLearn.value.replace(/\s/g, '')));
  const uniqueChars = Array.from(textToLearn.value.replace(/\s/g, ''));
  const pinyins = pinyin(textToLearn.value, { type: 'array' });
  console.log(pinyins);

  Array.from(textToLearn.value).forEach((char, index) => {
    charsMap.set(char, pinyins[index]);
  })
  console.log(charsMap);
  characters.value = uniqueChars;
  activeChar.value = ''; // 清空当前选中的汉字
  relatedInfo.value = null; // 清空相关信息
};

</script>

<template>
  <div class="min-h-screen bg-gray-100 text-gray-800 font-sans">
    <header class="bg-white shadow-md p-4">
      <h1 class="text-3xl font-bold text-center">{{ data.header1 }}</h1>
      <h2 class="text-2xl font-bold text-center">{{ data.header2 }}</h2>
    </header>
    <!-- <p class="font-bold">{{ data.question1 }}</p> -->
    <main class="p-4 md:p-8">
      <div class="max-w-4xl mx-auto">

        <!-- 汉字卡片区 -->
        <div class="bg-white p-6 rounded-lg shadow-lg mb-8">
          <h2 class="text-2xl mb-4">{{ data.question1 }}</h2>
          <div class="grid grid-cols-4 sm:grid-cols-6 md:grid-cols-8 lg:grid-cols-8 gap-4">
            <CharacterCard v-for="char in data.question1_data" :key="char" :character="char"
              :isActive="activeChar === char" @click="handleCardClick" />
          </div>
        </div>

        <!-- 汉字卡片区 -->
        <div class="bg-white p-6 rounded-lg shadow-lg mb-8">
          <h2 class="text-2xl mb-4">{{ data.question2 }}</h2>
          <div class="grid grid-cols-4 sm:grid-cols-4 md:grid-cols-4 lg:grid-cols-4 gap-4">
            <CharacterCard v-for="chars in data.question2_data" :key="chars" :character="chars"
              :isActive="activeChar === chars" @click="handleCardClick" />
          </div>
        </div>

        <!-- 组词/句子展示区 -->
        <div class="bg-white p-6 rounded-lg shadow-lg">
          <h2 class="text-2xl font-semibold mb-4">组词和句子</h2>
          <div v-if="activeChar" class="space-y-6">
            <div>
              <h3 class="text-xl font-semibold text-green-700 mb-3 border-b-2 border-green-200 pb-2">
                与“<span class="font-bold">{{ activeChar }}</span>”相关的词语
              </h3>
              <div v-if="relatedInfo && relatedInfo.words.length > 0" class="flex flex-wrap gap-3">
                <span v-for="word in relatedInfo.words" :key="word"
                  class="bg-green-100 text-green-800 text-lg px-4 py-2 rounded-full shadow-sm">
                  {{ word }}
                </span>
              </div>
              <p v-else class="text-gray-500">暂无相关词语。</p>
            </div>
            <div>
              <h3 class="text-xl font-semibold text-blue-700 mb-3 border-b-2 border-blue-200 pb-2">
                包含“<span class="font-bold">{{ activeChar }}</span>”的句子
              </h3>
              <div v-if="relatedInfo && relatedInfo.sentences.length > 0" class="space-y-3">
                <p v-for="sentence in relatedInfo.sentences" :key="sentence"
                  class="bg-blue-50 p-3 rounded-lg text-blue-800 text-md italic">
                  "{{ sentence }}"
                </p>
              </div>
              <p v-else class="text-gray-500">暂无相关句子。</p>
            </div>
          </div>
          <div v-else class="text-center text-gray-500 py-8">
            <p>点击上方的汉字后，这里会显示相关的组词或句子。</p>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

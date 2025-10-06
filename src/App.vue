<script setup lang="ts">
import CharacterCard from './components/CharacterCard.vue';

// const data = {
//   header1: '拼音大闯关 (3)',
//   header2: '【声母 b p m f】',
//   question1: "一、熟读下列音节。（尽量直呼，实在不行就拼读）",
//   question1_data: ['ba3',
//     'po4',
//     'mi4',
//     'fu2',
//     'bo3',
//     'mi1',
//     'pa1',
//     'pu3',

//     'po2',
//     'bi3',
//     'ma2',
//     'mu4',
//     'bi2',
//     'po1',
//     'fu1',
//     'fa4',

//     'bo1',
//     'pu4',
//     'mi3',
//     'fa2',
//     'bu4',
//     'ma3',
//     'pa4',
//     'pi1',

//     'pi2',
//     'bo2',
//     'mu3',
//     'mo4',
//     'bu3',
//     'pi1',
//     'fu3',
//     'fo2'],
//   question2: '二、熟读下列音节词。',
//   question2_data: [['pi2', 'fu1'], ['bo2', 'bo'], ['pi2', 'pa'], ['bo2', 'fu4'], ['fa2', 'mu4'], ['pa2', 'po1'], ['mu4', 'ma3'], ['fu3', 'mo1'], ['ma1', 'ma'], ['mu4', 'fa2'], ['mo2', 'fa3'], ['mu4', 'bu4'], ['fu4', 'mu3'], ['pu4', 'bu4'], ['bu4', 'fa2']],
// }

const data2 = {
  header1: '拼音大闯关 (4)',
  header2: '【声母 d t n l】',
  question1: "一、熟读下列音节。（尽量直呼，实在不行就拼读）",
  question1_data: [
    'da3',
    'li4',
    'tu2',
    'ni1',
    'la1',
    'tu3',
    'lv3',

    'ni3',
    'da2',
    'lu4',
    'ti2',
    'du1',
    'ta4',
    'nv3',

    'lu4',
    'di3',
    'na2',
    'tu4',
    'la3',
    'da4',
    'li1',

    'di2',
    'nu3',
    'lu3',
    'ti1',
    'du3',
    'ne2',
    'du2'],
  question2: '二、熟读下列音节词。',
  question2_data: [['di4', 'di'], ['da4', 'mi3'], ['ma3', 'lu4'], ['la4', 'bi3'], ['ma2', 'la4'], ['na3', 'li3'], ['nu3', 'li4'], ['ta4', 'bu4'], ['lv2', 'pi2'], ['da4', 'di4'], ['da3', 'ba3'], ['di4', 'tu2'], ['du4', 'pi2'], ['te4', 'di4'], ['ti2', 'mu4'], ['da4', 'lu4']],
}

// 用于节流的标志
let isSpeaking = false;

// 点击卡片时的回调
const handleCardClick = (char: string) => {
  if (isSpeaking) {
    console.log('正在发音，请稍后再试...');
    return;
  }
  // relatedInfo.value = dictionary[char] || null; // 从字典中获取数据

  // if (Array.isArray(char)) {
  //   char = char.join(' '); // 如果是数组，转换为字符串
  // }

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
</script>

<template>
  <div class="min-h-screen bg-gray-100 text-gray-800 font-sans">
    <header class="bg-white shadow-md p-4">
      <h1 class="text-3xl font-bold text-center">{{ data2.header1 }}</h1>
      <h2 class="text-2xl font-bold text-center">{{ data2.header2 }}</h2>
    </header>
    <!-- <p class="font-bold">{{ data.question1 }}</p> -->
    <main class="p-4 md:p-8">
      <div class="max-w-4xl mx-auto">

        <!-- 汉字卡片区 -->
        <div class="bg-white p-6 rounded-lg shadow-lg mb-8">
          <h2 class="text-2xl mb-4">{{ data2.question1 }}</h2>
          <div class="grid grid-cols-4 sm:grid-cols-6 md:grid-cols-8 lg:grid-cols-8 gap-4">
            <CharacterCard v-for="char in data2.question1_data" :key="char" :character="char"
              @click="handleCardClick" />
          </div>
        </div>

        <!-- 汉字卡片区 -->
        <div class="bg-white p-6 rounded-lg shadow-lg mb-8">
          <h2 class="text-2xl mb-4">{{ data2.question2 }}</h2>
          <div class="grid grid-cols-2 sm:grid-cols-4 md:grid-cols-4 lg:grid-cols-4 gap-4">
            <CharacterCard v-for="chars in data2.question2_data" :key="chars.join('-')" :character="chars"
              @click="handleCardClick" />
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

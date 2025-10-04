<script setup lang="ts">
import cnchar from 'cnchar';
import 'cnchar-idiom';
import 'cnchar-xhy';
import 'cnchar-draw';
import 'cnchar-words';
import 'cnchar-voice';

// Define props
const props = defineProps({
  character: {
    type: [String, Array],
    required: true,
  },
  isActive: {
    type: Boolean,
    default: false,
  },
});

setTimeout(() => {
  // console.log(cnchar.spellToWord('ba3'));
  // console.log(cnchar.xhy('你', 'fuzzy', 'second')); // ['醉汉过铁索桥', '扶着醉汉过破桥']
  // console.log(cnchar.idiom('你'));
  // console.log(cnchar.words('你'));
}, 1000);
// cnchar.draw('你好', {});

// Define emits
const emit = defineEmits(['click']);

// Generate pinyin for the character
// const pinyinText = computed(() => {
//   return pinyin(props.character, { toneType: 'symbol' });
// });

const handleClick = () => {
  // const pinyinText = Array.isArray(props.character)
  //   ? props.character.map(char => cnchar.shapeSpell(char)).join(' ')
  //   : cnchar.shapeSpell(props.character);
  // emit('click', pinyinText); // 格式：shuì zháo le
  emit('click', props.character); // 格式：shuì zháo le
};
</script>

<template>
  <div v-if="Array.isArray(character)" @click="handleClick"
    class="p-2 border rounded-lg text-center cursor-pointer transition-all duration-200 ease-in-out" :class="{
      'bg-green-200 shadow-lg scale-110': isActive,
      'bg-white hover:bg-green-100 hover:shadow-md': !isActive,
    }">
    <div class="text-2xl font-serif grid grid-cols-2 gap-2">
      <div>{{ cnchar.shapeSpell(character[0] as string) }}</div>
      <div>{{ cnchar.shapeSpell(character[1] as string) }}</div>
    </div>
    <!-- <div class="text-lg text-gray-500 mb-1">{{ cnchar.spellToWord(props.character.join(" ")) }}</div> -->
  </div>
  <div v-else @click="handleClick"
    class="p-2 border rounded-lg text-center cursor-pointer transition-all duration-200 ease-in-out" :class="{
      'bg-green-200 shadow-lg scale-110': isActive,
      'bg-white hover:bg-green-100 hover:shadow-md': !isActive,
    }">
    <div class="text-2xl font-serif">{{ cnchar.shapeSpell(character) }}</div>
    <!-- <div class="text-lg text-gray-500 mb-1">{{ cnchar.spellToWord(props.character)[0] }}</div> -->
  </div>
</template>

<style scoped></style>
<script setup lang="ts">
import { computed } from 'vue';
import { pinyin } from 'pinyin-pro';

// Define props
const props = defineProps<{
  character: string;
  isActive: boolean;
}>();

// Define emits
const emit = defineEmits(['click']);

// Generate pinyin for the character
const pinyinText = computed(() => {
  return pinyin(props.character, { toneType: 'symbol' });
});

const handleClick = () => {
  emit('click', props.character);
};
</script>

<template>
  <div
    @click="handleClick"
    class="p-2 border rounded-lg text-center cursor-pointer transition-all duration-200 ease-in-out"
    :class="{
      'bg-green-200 shadow-lg scale-110': isActive,
      'bg-white hover:bg-green-100 hover:shadow-md': !isActive,
    }"
  >
    <div class="text-lg text-gray-500 mb-1">{{ pinyinText }}</div>
    <div class="text-5xl font-serif">{{ character }}</div>
  </div>
</template>

<style scoped>
/* Additional styles can be added here if needed */
</style>

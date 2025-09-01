<script setup lang="ts">
import { ref } from 'vue';
import CharacterCard from './CharacterCard.vue';

// Ref for the textarea input
const textToLearn = ref('你好');

// Ref for the list of characters to display
const characters = ref<string[]>([]);

// Function to start the learning process
const startLearning = () => {
    // Remove duplicates and spaces, then split into an array of characters
    const uniqueChars = Array.from(new Set(textToLearn.value.replace(/\s/g, '')));
    characters.value = uniqueChars;
};

// Initialize with default text
startLearning();
</script>

<template>
    <div class="min-h-screen bg-gray-100 text-gray-800 font-sans">
        <header class="bg-white shadow-md p-4">
            <h1 class="text-3xl font-bold text-center text-green-600">拼音识字 (Pinyin Literacy)</h1>
        </header>

        <main class="p-4 md:p-8">
            <div class="max-w-4xl mx-auto">
                <!-- Input Section -->
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

                <!-- Character Grid Section -->
                <div class="bg-white p-6 rounded-lg shadow-lg mb-8">
                    <h2 class="text-2xl font-semibold mb-4">点击汉字开始发音</h2>
                    <div v-if="characters.length > 0"
                        class="grid grid-cols-4 sm:grid-cols-6 md:grid-cols-8 lg:grid-cols-10 gap-4">
                        <!-- Character cards will go here -->
                        <div v-for="char in characters" :key="char"
                            class="p-2 border rounded-lg text-center cursor-pointer hover:bg-green-100 hover:shadow-md transition-all">
                            <span class="text-4xl">{{ char }}</span>
                        </div>
                    </div>
                    <p v-else class="text-gray-500">请输入一些汉字以开始学习。</p>
                </div>

                <!-- Word/Sentence Display Section -->
                <div class="bg-white p-6 rounded-lg shadow-lg">
                    <h2 class="text-2xl font-semibold mb-4">组词/句子</h2>
                    <div class="text-center text-gray-500">
                        <p>点击上方的汉字后，这里会显示相关的组词或句子。</p>
                    </div>
                </div>
            </div>
        </main>
    </div>
</template>

<style scoped>
/* Scoped styles can be added here if needed, but most styling is done with Tailwind CSS. */
</style>
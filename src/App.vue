<template>
  <div class="min-h-screen py-10 px-4 bg-slate-100">
    <div class="max-w-5xl mx-auto space-y-6">

      <!-- TITLE -->
      <h1 class="text-4xl font-bold text-gray-800">
        ✨ Case Converter
      </h1>

      <!-- INPUT CARD -->
      <div class="bg-white rounded-xl shadow border border-gray-200 p-4">
        
        <h2 class="font-semibold text-gray-700 mb-2">Input Text</h2>

        <textarea
          v-model="inputText"
          placeholder="Type or paste your content here..."
          class="w-full h-52 p-3 border rounded-lg resize-none outline-none focus:ring-2 focus:ring-blue-400"
        ></textarea>

        <!-- CONVERT BUTTONS -->
        <div class="flex flex-wrap gap-2 mt-4">
          <button @click="toSentence" class="btn">Sentence</button>
          <button @click="toLower" class="btn">lower</button>
          <button @click="toUpper" class="btn">UPPER</button>
          <button @click="toTitle" class="btn">Title</button>
          <button @click="toAlt" class="btn">aLtErNaTiNg</button>
          <button @click="toInverse" class="btn">iNVERSE</button>
        </div>

        <!-- INFO -->
        <div class="text-sm text-gray-500 mt-3">
          Character: {{ charCount }} | Word: {{ wordCount }} | Line: {{ lineCount }}
        </div>
      </div>

      <!-- RESULT CARD -->
      <div class="bg-white rounded-xl shadow border border-gray-200 p-4">

        <div class="flex justify-between items-center mb-2">
          <h2 class="font-semibold text-gray-700">Result</h2>

          <!-- TOOLS (PINDAH KE SINI) -->
          <div class="flex gap-2">
            <button @click="copyText" class="tool">Copy</button>
            <button @click="downloadText" class="tool">Download</button>
            <button @click="clearText" class="tool-danger">Clear</button>
          </div>
        </div>

        <div class="bg-gray-50 p-4 rounded-lg min-h-[120px]">
          <p class="whitespace-pre-wrap text-gray-800">
            {{ outputText || 'Result will appear here...' }}
          </p>
        </div>

      </div>

    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const inputText = ref('')
const outputText = ref('')

// ===== CONVERT =====
const toUpper = () => outputText.value = inputText.value.toUpperCase()
const toLower = () => outputText.value = inputText.value.toLowerCase()

const toTitle = () => {
  outputText.value = inputText.value
    .toLowerCase()
    .split(' ')
    .map(w => w.charAt(0).toUpperCase() + w.slice(1))
    .join(' ')
}

const toSentence = () => {
  const t = inputText.value.toLowerCase()
  outputText.value = t.charAt(0).toUpperCase() + t.slice(1)
}

const toAlt = () => {
  outputText.value = inputText.value
    .split('')
    .map((c, i) => i % 2 ? c.toLowerCase() : c.toUpperCase())
    .join('')
}

const toInverse = () => {
  outputText.value = inputText.value
    .split('')
    .map(c => c === c.toUpperCase() ? c.toLowerCase() : c.toUpperCase())
    .join('')
}

// ===== TOOLS =====
const clearText = () => {
  inputText.value = ''
  outputText.value = ''
}

const copyText = () => {
  navigator.clipboard.writeText(outputText.value)
}

const downloadText = () => {
  const blob = new Blob([outputText.value], { type: 'text/plain' })
  const url = URL.createObjectURL(blob)

  const a = document.createElement('a')
  a.href = url
  a.download = 'result.txt'
  a.click()

  URL.revokeObjectURL(url)
}

// ===== COUNTER =====
const charCount = computed(() => inputText.value.length)

const wordCount = computed(() => {
  const text = inputText.value.trim()
  return text ? text.split(/\s+/).length : 0
})

const lineCount = computed(() => {
  if (!inputText.value) return 0
  return inputText.value.split('\n').length
})
</script>

<style>
.btn {
  background: #f1f5f9;
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 14px;
  transition: 0.2s;
}
.btn:hover {
  background: #3b82f6;
  color: white;
}

.tool {
  background: #e5e7eb;
  padding: 4px 10px;
  border-radius: 6px;
  font-size: 13px;
}
.tool:hover {
  background: #d1d5db;
}

.tool-danger {
  background: #fee2e2;
  padding: 4px 10px;
  border-radius: 6px;
  font-size: 13px;
}
.tool-danger:hover {
  background: #fecaca;
}
</style>
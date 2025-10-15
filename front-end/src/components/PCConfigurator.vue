<!-- src/components/PCConfigurator.vue -->
<template>
  <div>
    <div class="grid grid-cols-8 gap-4 items-start">
      <!-- Gallery: займає 3 колонки -->
      <div class="col-span-3">
        <Gallery v-if="images.length" :images="images" />
        <div v-else class="text-center text-gray-500">
          Loading images...
        </div>
      </div>

      <!-- Селектори CPU, RAM, GPU, Case: 2 колонки -->
      <div class="col-span-2 space-y-4">
        <!-- CPU Selector -->
        <CPUSelector @selectCpu="handleCpuSelect" />

        <!-- RAM Selector -->
        <RAMSelector @selectRam="handleRamSelect" />

        <!-- GPU Selector -->
        <GPUSelector @selectGpu="handleGpuSelect" />

        <!-- Case Selector -->
        <CASESelector @selectCase="handleCaseSelect" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import Gallery from './Gallery.vue'
import CPUSelector from './CPUSelector.vue'
import RAMSelector from './RAMSelector.vue'
import GPUSelector from './GPUSelector.vue'
import CASESelector from './CASESelector.vue'

// --- Галерея ---
const images = ref([])

// Для початку можна завантажити якісь дефолтні зображення або лишити пустим
onMounted(() => {
  // Наприклад, дефолтні зображення загальної галереї:
  images.value = [
    {
      itemImageSrc: new URL('../assets/images/1.webp', import.meta.url).href,
      thumbnailImageSrc: new URL('../assets/images/1.webp', import.meta.url).href,
      alt: 'Photo 1'
    },
    {
      itemImageSrc: new URL('../assets/images/2.webp', import.meta.url).href,
      thumbnailImageSrc: new URL('../assets/images/2.webp', import.meta.url).href,
      alt: 'Photo 2'
    },
    {
      itemImageSrc: new URL('../assets/images/3.webp', import.meta.url).href,
      thumbnailImageSrc: new URL('../assets/images/3.webp', import.meta.url).href,
      alt: 'Photo 3'
    },
    {
      itemImageSrc: new URL('../assets/images/4.webp', import.meta.url).href,
      thumbnailImageSrc: new URL('../assets/images/4.webp', import.meta.url).href,
      alt: 'Photo 4'
    }
  ]
})

// --- Підготовка до динамічного імпорту зображень корпусів ---
// Використовуємо Vite import.meta.glob для всіх зображень у підпапках cases:
// Припускаємо, що структуру маєте так: src/assets/images/cases/<CaseName>/*.jpg/png/webp
// Шлях тут відносний до цього файлу: компонент у src/components, тому '../assets/images/cases/...'
const caseImageModules = import.meta.glob(
  '../assets/images/cases/*/*.{jpg,jpeg,png,webp}',
  { eager: true, as: 'url' }
)
// caseImageModules: об'єкт, ключі — відносні шляхи до файлів, значення — URL-рядок готовий до завантаження

// --- Стан виборів ---
const selectedCpu = ref(null)
const selectedRam = ref(null)
const selectedGpu = ref(null)
const selectedCase = ref(null)

// Обробники подій від селекторів
function handleCpuSelect(val) {
  selectedCpu.value = val
  console.log('Selected CPU in PCConfigurator:', val)
}

function handleRamSelect(val) {
  selectedRam.value = val
  console.log('Selected RAM in PCConfigurator:', val)
}

function handleGpuSelect(val) {
  selectedGpu.value = val
  console.log('Selected GPU in PCConfigurator:', val)
}

function handleCaseSelect(val) {
  selectedCase.value = val
  console.log('Selected Case in PCConfigurator:', val)

  // При виборі корпуса оновлюємо галерею: беремо зображення з папки cases/<val>/
  // Фільтруємо caseImageModules за шляхом, що містить назву вибраного корпуса
  const matched = []
  for (const path in caseImageModules) {
    // Перевіряємо, чи шлях містить '/cases/<назва>/'.
    // Зверніть увагу на регістр: назва папки повинна точно відповідати рядку val.
    // Якщо папка названа інакше (наприклад, без пробілів), потрібно привести val у відповідний формат.
    if (path.includes(`/cases/${val}/`)) {
      const url = caseImageModules[path]
      // Формуємо об’єкт формату галереї
      matched.push({
        itemImageSrc: url,
        thumbnailImageSrc: url,
        alt: `${val} image`
      })
    }
  }

  if (matched.length) {
    images.value = matched
  } else {
    // Якщо не знайдено жодного зображення для цього корпуса, можна очистити галерею або повідомити
    console.warn(`No images found for case folder: ${val}`)
    images.value = []
  }
}
</script>

<style scoped>
/* Додаткові стилі при потребі */
</style>

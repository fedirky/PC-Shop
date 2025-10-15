<!-- src/components/FileLoader.vue -->
<template>
  <div class="card p-4" style="padding: 0 !important;">
    <!-- Toast container -->
    <Toast />

    <FileUpload
      name="demo[]"
      url="/api/upload"
      :multiple="true"
      accept="image/*"
      :maxFileSize="1000000"
      @upload="onTemplatedUpload"
      @select="onSelectedFiles"
      @clear="onClearTemplatingUpload"
    >
      <!-- Шапка кастомізованого FileUpload -->
      <template #header="{ chooseCallback, uploadCallback, clearCallback, files }">
        <div class="flex flex-col gap-2 w-full">
          <!-- Рядок кнопок -->
          <div class="flex gap-2">
            <Button
              class="w-12 h-12"
              @click="chooseCallback()"
              rounded
              outlined
              severity="secondary"
            >
              <PhotoIcon class="h-6 w-6" />
            </Button>
            <Button
              class="w-12 h-12"
              @click="uploadEvent(uploadCallback)"
              rounded
              outlined
              severity="success"
              :disabled="!files || files.length === 0"
            >
              <CloudArrowUpIcon class="h-6 w-6" />
            </Button>
            <Button
              class="w-12 h-12"
              @click="clearEvent(clearCallback)"
              rounded
              outlined
              severity="danger"
              :disabled="!files || files.length === 0"
            >
              <XMarkIcon class="h-6 w-6" />
            </Button>
          </div>
          <!-- Шкала прогресу -->
          <div class="w-full">
            <ProgressBar
              :value="totalSizePercent"
              :showValue="false"
              class="w-full h-1"
            >
              <span class="whitespace-nowrap">{{ formattedTotalSize }} / 1 MB</span>
            </ProgressBar>
          </div>
        </div>
      </template>

      <!-- Контент: Pending та Completed -->
      <template #content="{ files, uploadedFiles, removeUploadedFileCallback, removeFileCallback }">
        <div class="flex flex-col gap-8 pt-4">
          <!-- Pending -->
          <div v-if="files && files.length > 0">
            <h5>Pending</h5>
            <div class="flex flex-wrap gap-4">
              <div
                v-for="(file, index) in files"
                :key="file.name + file.type + file.size"
                class="p-4 rounded flex flex-col border border-gray-200 items-center gap-2"
              >
                <div>
                  <img
                    role="presentation"
                    :alt="file.name"
                    :src="file.objectURL"
                    width="100"
                    height="50"
                  />
                </div>
                <span class="font-semibold text-ellipsis max-w-60 whitespace-nowrap overflow-hidden">
                  {{ file.name }}
                </span>
                <div>{{ formatSize(file.size) }}</div>
                <Badge value="Pending" severity="warn" />
                <Button
                  @click="onRemoveTemplatingFile(file, removeFileCallback, index)"
                  rounded
                  outlined
                  severity="danger"
                >
                  <XMarkIcon class="h-5 w-5" />
                </Button>
              </div>
            </div>
          </div>

          <!-- Completed -->
          <div v-if="uploadedFiles && uploadedFiles.length > 0">
            <h5>Completed</h5>
            <div class="flex flex-wrap gap-4">
              <div
                v-for="(file, index) in uploadedFiles"
                :key="file.name + file.type + file.size"
                class="p-4 rounded flex flex-col border border-gray-200 items-center gap-2"
              >
                <div>
                  <img
                    role="presentation"
                    :alt="file.name"
                    :src="file.objectURL"
                    width="100"
                    height="50"
                  />
                </div>
                <span class="font-semibold text-ellipsis max-w-60 whitespace-nowrap overflow-hidden">
                  {{ file.name }}
                </span>
                <div>{{ formatSize(file.size) }}</div>
                <Badge value="Completed" class="mt-2" severity="success" />
                <Button
                  @click="removeUploadedFileCallback(index)"
                  rounded
                  outlined
                  severity="danger"
                >
                  <XMarkIcon class="h-5 w-5" />
                </Button>
              </div>
            </div>
          </div>
        </div>
      </template>

      <!-- Empty state -->
      <template #empty>
        <div class="flex items-center justify-center flex-col p-8">
          <CloudArrowUpIcon class="border-2 rounded-full p-4 text-gray-400 h-20 w-20" />
          <p class="mt-6 mb-0 text-gray-500">Drag and drop files here to upload.</p>
        </div>
      </template>
    </FileUpload>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import FileUpload from 'primevue/fileupload'
import Button from 'primevue/button'
import ProgressBar from 'primevue/progressbar'
import Badge from 'primevue/badge'
import Toast from 'primevue/toast'
import { useToast } from 'primevue/usetoast'
import { usePrimeVue } from 'primevue/config'

// Іконки Heroicons
import { PhotoIcon, CloudArrowUpIcon, XMarkIcon } from '@heroicons/vue/24/solid'

// Сервіс Toast
const toast = useToast()
// Доступ до конфігурації PrimeVue (для форматування розміру файлу)
const primevue = usePrimeVue()

// Максимальний розмір у байтах (1 MB)
const MAX_FILE_SIZE = 1_000_000

// Стан: загальний розмір pending-файлів у байтах
const totalSize = ref(0)

// Обчислений відсоток для ProgressBar (0-100)
const totalSizePercent = computed(() => {
  return totalSize.value > 0
    ? Math.min((totalSize.value / MAX_FILE_SIZE) * 100, 100)
    : 0
})

// Обчислений відформатований загальний розмір (в людському вигляді)
const formattedTotalSize = computed(() => formatSize(totalSize.value))

// Обробка вибору файлів
const onSelectedFiles = (event) => {
  totalSize.value = 0
  if (event.files && event.files.length) {
    event.files.forEach((file) => {
      totalSize.value += file.size
    })
  }
}

// Обробник видалення окремого pending-файлу
const onRemoveTemplatingFile = (file, removeFileCallback, index) => {
  removeFileCallback(index)
  totalSize.value = Math.max(totalSize.value - file.size, 0)
}

// Обробка очищення списку pending-файлів
const onClearTemplatingUpload = () => {
  totalSize.value = 0
}

// Якщо хочемо окремий метод для кнопки clear у header:
const clearEvent = (clearCallback) => {
  clearCallback()
  totalSize.value = 0
}

// Обробник завантаження (upload)
const uploadEvent = (uploadCallback) => {
  uploadCallback()
}

// Після завершення upload // TODO: Зробити цю частину коду робочою.
const onTemplatedUpload = () => {
  toast.add({
    severity: 'info',
    summary: 'Success',
    detail: 'File uploaded',
    life: 3000
  })
  totalSize.value = 0
}

// Форматування розміру файлу у людський вигляді
function formatSize(bytes) {
  const k = 1024
  const dm = 2
  const sizes = primevue.config.locale?.fileSizeTypes || ['B', 'KB', 'MB', 'GB', 'TB']
  if (!bytes) {
    return `0 ${sizes[0]}`
  }
  const i = Math.floor(Math.log(bytes) / Math.log(k))
  const formattedSize = parseFloat((bytes / Math.pow(k, i)).toFixed(dm))
  return `${formattedSize} ${sizes[i]}`
}
</script>

<style scoped>
/* Додаткові стилі за потреби */
</style>

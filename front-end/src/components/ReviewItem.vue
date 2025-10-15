<!-- src/components/ReviewItem.vue -->
<template>
  <div class="w-full flex space-x-4">
    <!-- Avatar: якщо є image, показуємо її; інакше — placeholder з ініціалами -->
    <Avatar
      v-if="avatar"
      :image="avatar"
      shape="circle"
      size="large"
      class="flex-shrink-0"
    />
    <Avatar
      v-else
      :label="initials"
      shape="circle"
      size="large"
      class="flex-shrink-0 bg-gray-300 text-gray-700"
    />

    <!-- Контент огляду -->
    <div class="flex-1">
      <!-- Ім'я рецензента та рейтинг -->
      <div class="flex items-center justify-between">
        <span class="font-semibold text-gray-800">{{ reviewer }}</span>
        <Rating
          :modelValue="rating"
          :stars="5"
          :cancel="false"
          disabled
          class="ml-2 text-yellow-500"
        />
      </div>

      <!-- Дата огляду -->
      <p v-if="date" class="text-xs text-gray-500 mt-1">{{ formattedDate }}</p>

      <!-- Текст огляду -->
      <div class="mt-2 text-gray-700 whitespace-pre-wrap">
        {{ reviewText }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import Avatar from 'primevue/avatar'
import Rating from 'primevue/rating'

const props = defineProps({
  avatar: {
    type: String,
    default: '' // якщо порожній, показуємо placeholder
  },
  reviewer: {
    type: String,
    required: true
  },
  rating: {
    type: Number,
    default: 0
  },
  reviewText: {
    type: String,
    required: true
  },
  date: {
    type: [String, Date],
    default: null
  }
})

// Обчислюємо ініціали з reviewer, наприклад "John Doe" -> "JD"
const initials = computed(() => {
  if (!props.reviewer) return ''
  const parts = props.reviewer.trim().split(/\s+/)
  if (parts.length === 0) return ''
  if (parts.length === 1) {
    return parts[0].charAt(0).toUpperCase()
  }
  const first = parts[0].charAt(0).toUpperCase()
  const last = parts[parts.length - 1].charAt(0).toUpperCase()
  return first + last
})

// Форматуємо дату як DD-MM-YYYY
const formattedDate = computed(() => {
  if (!props.date) return ''
  const d = typeof props.date === 'string' ? new Date(props.date) : props.date
  if (!(d instanceof Date) || isNaN(d)) return ''
  const day = String(d.getDate()).padStart(2, '0')
  const month = String(d.getMonth() + 1).padStart(2, '0')
  const year = d.getFullYear()
  return `${day}-${month}-${year}`
})
</script>

<style scoped>
/* Ніяких додаткових стилів тут не потрібно, усі утиліти наведені в класах */
</style>

<!-- src/components/ReviewSection.vue -->
<template>
  <div class="card">
    <DataView :value="reviews" layout="list" paginator :rows="5" style="padding-top: 0px !important">
      <template #list="slotProps">
        <div class="flex flex-col w-full">
          <div
            v-for="(item, index) in slotProps.items"
            :key="index"
            class="w-full"
            :class="index !== 0 ? 'border-t border-gray-200' : ''"
          >
            <div class="py-4">
              <ReviewItem
                :avatar="item.avatar"
                :reviewer="item.reviewer"
                :rating="item.rating"
                :reviewText="item.reviewText"
                :date="item.date"
              />
            </div>
          </div>
        </div>
      </template>
    </DataView>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import DataView from 'primevue/dataview'
import ReviewItem from './ReviewItem.vue'

// Дані оглядів; замініть на реальні або динамічні дані
const reviews = ref([
  {
    avatar: 'https://primefaces.org/cdn/primevue/images/avatar/amyelsner.png',
    reviewer: 'AmyElsner',
    rating: 4,
    reviewText: 'Very convenient shop, I liked everything! Highly recommended.',
    date: '2025-06-12T14:30:00'
  },
  {
    avatar: '',
    reviewer: 'User123',
    rating: 5,
    reviewText: 'Fast delivery and excellent PC quality.',
    date: '2025-06-10'
  },
  {
    avatar: '',
    reviewer: 'AnotherUser',
    rating: 3,
    reviewText: 'It was okay, but could be better.',
    date: '2025-06-08'
  },
  {
    avatar: 'https://primefaces.org/cdn/primevue/images/avatar/onyamalimba.png',
    reviewer: 'Onyamalimba',
    rating: 5,
    reviewText: 'Excellent service and quality!',
    date: '2025-06-05'
  },
  {
    avatar: '',
    reviewer: 'TestUser',
    rating: 2,
    reviewText: 'Not satisfied with delivery time.',
    date: '2025-06-02'
  },
  {
    avatar: '',
    reviewer: 'ExtraUser',
    rating: 4,
    reviewText: 'Good overall, will buy again.',
    date: '2025-05-30'
  }
])
</script>

<style scoped>
/* Tailwind надає border-gray-200; якщо потрібно інший відтінок, можна перевизначити */
</style>

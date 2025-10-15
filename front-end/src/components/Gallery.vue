<template>
  <div class="card">
    <Galleria
      v-if="images && images.length"
      :value="images"
      :responsiveOptions="responsiveOptions"
      :numVisible="5"
      :circular="true"
      containerStyle="max-width: 1000px"
      :showItemNavigators="false"
      :showThumbnails="false"
      :showItemNavigatorsOnHover="true"
      :showIndicators="true"
    >
      <template #item="slotProps">
        <!-- Квадратний контейнер з білим фоном -->
        <div
          class="w-full"
          style="aspect-ratio: 1/1; background: white; display: flex; align-items: center; justify-content: center;"
        >
          <img
            :src="slotProps.item.itemImageSrc"
            :alt="slotProps.item.alt"
            style="max-width: 100%; max-height: 100%; object-fit: contain;"
          />
        </div>
      </template>
      <!-- слот thumbnail залишається, але showThumbnails=false, отже не видно -->
      <template #thumbnail="slotProps">
        <div
          class="w-full"
          style="aspect-ratio: 1/1; background: white; display: flex; align-items: center; justify-content: center;"
        >
          <img
            :src="slotProps.item.thumbnailImageSrc"
            :alt="slotProps.item.alt"
            style="max-width: 100%; max-height: 100%; object-fit: contain;"
          />
        </div>
      </template>
    </Galleria>
    <div v-else class="text-center text-gray-500">
      Немає зображень для відображення.
    </div>
  </div>
</template>

<script setup>
import { toRef } from 'vue'
import Galleria from 'primevue/galleria'

// Проп: масив локальних зображень
const props = defineProps({
  images: {
    type: Array,
    default: () => []
  }
})
const images = toRef(props, 'images')

const responsiveOptions = [
  { breakpoint: '991px', numVisible: 4 },
  { breakpoint: '767px', numVisible: 3 },
  { breakpoint: '575px', numVisible: 1 }
]
</script>


<style>
</style>

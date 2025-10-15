<!-- src/components/RAMSelector.vue -->
<template>
  <UniversalSelector
    defaultLabel="Select RAM"
    prefix="RAM"
    icon="pi pi-fw pi-memory"
    :options="ramOptions"
    @select="onSelect"
  />
</template>

<script setup>
import { defineEmits } from 'vue'
import UniversalSelector from './UniversalSelector.vue'

const emit = defineEmits(['selectRam'])

const ramOptions = [
  { label: '16 GB', value: '16 GB', icon: 'pi pi-fw pi-angle-right' },
  { label: '32 GB', value: '32 GB', icon: 'pi pi-fw pi-angle-right' },
  { label: '64 GB', value: '64 GB', icon: 'pi pi-fw pi-angle-right' }
]

function onSelect(value) {
  emit('selectRam', value)
}
</script>

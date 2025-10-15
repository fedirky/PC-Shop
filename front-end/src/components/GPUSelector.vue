<!-- src/components/GPUSelector.vue -->
<template>
  <UniversalSelector
    defaultLabel="Select GPU"
    prefix="GPU"
    icon="pi pi-fw pi-desktop"
    :options="gpuOptions"
    @select="onSelect"
  />
</template>

<script setup>
import { defineEmits } from 'vue'
import UniversalSelector from './UniversalSelector.vue'

const emit = defineEmits(['selectGpu'])

const gpuOptions = [
  {
    label: 'NVIDIA',
    icon: 'pi pi-fw pi-microchip',
    children: [
      { label: 'GeForce RTX 4090', value: 'GeForce RTX 4090', icon: 'pi pi-fw pi-angle-right' },
      { label: 'GeForce RTX 4080', value: 'GeForce RTX 4080', icon: 'pi pi-fw pi-angle-right' },
      { label: 'GeForce RTX 4070', value: 'GeForce RTX 4070', icon: 'pi pi-fw pi-angle-right' }
    ]
  },
  {
    label: 'AMD',
    icon: 'pi pi-fw pi-microchip',
    children: [
      { label: 'Radeon RX 7900 XT', value: 'Radeon RX 7900 XT', icon: 'pi pi-fw pi-angle-right' },
      { label: 'Radeon RX 7800 XT', value: 'Radeon RX 7800 XT', icon: 'pi pi-fw pi-angle-right' },
      { label: 'Radeon RX 7600',   value: 'Radeon RX 7600',   icon: 'pi pi-fw pi-angle-right' }
    ]
  },
  {
    label: 'Intel',
    icon: 'pi pi-fw pi-microchip',
    children: [
      { label: 'Arc A770', value: 'Arc A770', icon: 'pi pi-fw pi-angle-right' },
      { label: 'Arc A580', value: 'Arc A580', icon: 'pi pi-fw pi-angle-right' },
      { label: 'Arc A380', value: 'Arc A380', icon: 'pi pi-fw pi-angle-right' }
    ]
  }
]

function onSelect(value) {
  emit('selectGpu', value)
}
</script>


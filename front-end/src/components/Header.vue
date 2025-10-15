<template>
  <div class="card">
    <Toolbar
      class="rounded-full py-4 px-6"
      style="border-radius: 0px !important"
    >
      <!-- Ліва частина тулбара -->
      <template #start>
        <div class="flex items-center gap-4">
          <img
            :src="logoUrl"
            alt="PC Shop Logo"
            class="h-10 w-auto"
          />
          <span class="text-xl font-bold text-gray-800">PC Shop</span>
        </div>
      </template>

      <!-- Центральна частина тулбара -->
      <template #center>
        <div class="w-full max-w-md">
          <input
            type="text"
            placeholder="Search products..."
            class="border border-gray-300 rounded-full px-4 py-2 w-full focus:outline-none focus:ring-2 focus:ring-violet-500"
          />
        </div>
      </template>

      <!-- Права частина тулбара -->
      <template #end>
        <div class="flex items-center gap-4">
          <button @click="handleNotifications" class="p-2 rounded-full hover:bg-gray-100">
            <QueueListIcon class="w-6 h-6 text-gray-700" />
          </button>
          <button @click="handleNotifications" class="p-2 rounded-full hover:bg-gray-100">
            <BellIcon class="w-6 h-6 text-gray-700" />
          </button>
          <!-- Іконка кошика -->
          <button @click="goToCart" class="relative p-2 rounded-full hover:bg-gray-100">
            <ShoppingCartIcon class="w-6 h-6 text-gray-700" />
            <!-- за потреби бейдж для кількості товарів -->
            <!--
            <span class="absolute top-0 right-0 inline-flex items-center justify-center px-1.5 py-0.5 text-xs font-bold leading-none text-white bg-violet-500 rounded-full">
              3
            </span>
            -->
          </button>
          <Button
            label="Options"
            icon="pi pi-chevron-down"
            text
            plain
            @click="handleOptions"
          />
          <Avatar
            image="https://primefaces.org/cdn/primevue/images/avatar/amyelsner.png"
            style="width: 32px; height: 32px"
          />
          <!-- Іконка сповіщень -->
        </div>
      </template>
    </Toolbar>
  </div>
</template>

<script setup>
import Toolbar from 'primevue/toolbar'
import Button from 'primevue/button'
import Avatar from 'primevue/avatar'
import { BellIcon, ShoppingCartIcon, QueueListIcon} from '@heroicons/vue/24/solid'

// Підключення власного лого через import.meta.url
const logoUrl = new URL('../assets/images/logo1.png', import.meta.url).href

function handleOptions() {
  console.log('Options clicked')
  // логіка для Options
}
function handleNotifications() {
  console.log('Notifications clicked')
  // відкрити сповіщення
}
function goToCart() {
  console.log('Go to cart')
  // router.push('/cart') або інша логіка
}
</script>
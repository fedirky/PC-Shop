<!-- src/components/TabsMenu.vue -->
<template>
  <div class="card p-4 pt-0">
    <Tabs v-model:value="activeTab">
      <TabList>
        <Tab
          v-for="tab in tabs"
          :key="tab.value"
          :value="tab.value"
        >{{ tab.title }}</Tab>
      </TabList>
      <TabPanels>
        <TabPanel
          v-for="tab in tabs"
          :key="tab.value"
          :value="tab.value"
        >
          <div>
            <component :is="tab.component" />
          </div>
        </TabPanel>
      </TabPanels>
    </Tabs>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Tabs from 'primevue/tabs'
import TabList from 'primevue/tablist'
import Tab from 'primevue/tab'
import TabPanels from 'primevue/tabpanels'
import TabPanel from 'primevue/tabpanel'

import PCConfigurator from './PCConfigurator.vue'
import ReviewSection from './ReviewSection.vue'
import QuestionsComponent from './QuestionsComponent.vue'

// Register PrimeVue tab components locally
const components = { Tabs, TabList, Tab, TabPanels, TabPanel }

// Active tab: initialize to '0' to select first tab by default
const activeTab = ref('0')

// Define tabs: title shown in header, value used by Tabs, and component to render
const tabs = ref([
  { title: 'Product', value: '0', component: PCConfigurator },
  { title: 'Revievs', value: '1', component: ReviewSection },
  { title: 'Questions', value: '2', component: QuestionsComponent }
])
</script>

<style scoped>
/* Adjust as needed. .card class from PrimeVue or your styles provides background/border. */
</style>



<script setup>
import { ref } from 'vue';
import TheWelcome from '../components/TheWelcome.vue'
import UploaderComponent from '@/components/UploaderComponent.vue';
import SuscriptionsComponent from '@/components/SuscriptionsComponent.vue';
import DashboardComponent from '@/components/DashboardComponent.vue';
import CalendarComponent from '@/components/CalendarComponent.vue';

const subscriptions = ref([]);

// Get the emits from uploadesComponent
const getSubscriptions = (emit) => {
  subscriptions.value = emit;
}



</script>

<template>
  <main>
    <!-- <TheWelcome /> -->
    <UploaderComponent @dataUploaded="getSubscriptions" />
    <SuscriptionsComponent :subscriptions="subscriptions" />
    <DashboardComponent :subscriptions="subscriptions"  />
    <CalendarComponent :subscriptions="subscriptions" />
  </main>
</template>

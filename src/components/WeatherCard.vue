<script setup>
import WeatherForcastDay from './WeatherForcastDay.vue'
import BorderLine from './BorderLine.vue'
import WeatherInfo from './WeatherInfo.vue'
import { ref } from 'vue'
defineProps({
  place: Object
})
const showDetail = ref(false)
const emit = defineEmits(['delete-place'])
const removePlace = (placeName) => {
  emit('delete-place', placeName)
  showDetail.value = false
}
</script>

<template>
  <div class="text-white p-10 rounded-lg shadow-lg gap-6 mb-6 relative overflow-hidden bg-blue-500">
    <div class="mb-2 flex justify-between items-center">
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-location-dot"></i>
        <h1 class="text-3xl">{{ place.location.name }}</h1>
      </div>
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-clock"></i>
        <h1 class="text-3xl">
          {{ new Date(place.location.localtime).getHours() }}:{{
            new Date(place.location.localtime).getMinutes()
          }}
        </h1>
      </div>
    </div>

    <div class="text-center flex-1">
      <img :src="place.current.condition.icon" alt="icon" width="200" class="mx-auto -mb-10" />
      <h1 class="text-9xl mb-2">{{ Math.round(place.current.temp_c) }}&deg;</h1>
      <p class="text-2xl">{{ place.current.condition.text }}</p>
    </div>

    <BorderLine />

    <div v-for="(day, idx) in place.forecast.forecastday" :key="idx">
      <WeatherForcastDay :day="day" />
    </div>

    <div v-show="showDetail">
      <WeatherInfo
        :place="place"
        @close-info="showDetail = false"
        @remove-place="removePlace(place.location.name)"
      />
    </div>

    <div class="flex justify-end items-center gap-1 mt-10">
      <button @click="showDetail = true">
        More <i class="fa-solid fa-arrow-right text-sm -mb-px"></i>
      </button>
    </div>
  </div>
</template>

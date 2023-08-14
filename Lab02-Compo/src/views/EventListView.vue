<script setup lang="ts">
import EventCard from '../components/EventCard.vue'
import NewComponent from '@/components/NewComponent.vue'
import type { EventItem } from '@/type'
import { ref, type Ref, watchEffect, computed  } from 'vue'
import EventService from '@/services/EventService'
import type { AxiosResponse } from 'axios'
import { onBeforeRouteUpdate, useRouter } from 'vue-router'
import NProgress from 'nprogress'

const events: Ref<Array<EventItem>> = ref([])
const router = useRouter()
const totalEvent = ref<number>(0)
const props = defineProps({
  page: {
    type: Number,
    required: true
  } ,
  limit: {
    type: Number,
    required: true
  }
})


EventService.getEvent(3, props.page).then((response: AxiosResponse<EventItem[]>) => {
  events.value = response.data
  totalEvent.value = response.headers['x-total-count']
}).catch(() => {
  router.push({ name: 'NetworkError' })
})

onBeforeRouteUpdate((to, from, next) => {
  const toPage = Number(to.query.page)
  EventService.getEvent(3, toPage).then((response: AxiosResponse<EventItem[]>) => {
    events.value = response.data
    totalEvent.value = response.headers['x-total-count']
    next()
  }).catch(() => {
    next({ name: 'NetworkError' })
  })
})

const hasNextPage = computed(() => {
  const totalPages = Math.ceil(totalEvent.value / 3)
  return props.page.valueOf() < totalPages
})
</script>

<template>
  <h1>Events For Greater Good</h1>
  <main class="flex flex-col items-center">
    <EventCard v-for="event in events" :key="event.id" :event="event"></EventCard>
    <NewComponent v-for="event in events" :key="event.id" :event="event"></NewComponent>

  <div class="flex w-72 justify-between">
    <RouterLink :to="{ name: 'EventList', query: {page: page -1}}" rel="prev" v-if="page!= 1" id="page-prev" class="text-left text-gray-700 no-underline">
      Prev Page
    </RouterLink>

    <RouterLink :to="{name:'EventList', query:{page: page + 1} }" rel="next" v-if="hasNextPage" id="page-next" class="text-right text-gray-700 no-underline">
    Next Page
    </RouterLink>
  </div>

  </main>
</template>

<template>
  <div>
    <Suspense>
      <AsyncCityView/>
      <template #fallback>
        <Loading/>
      </template>
    </Suspense>
  </div>
</template>

<script lang="ts" setup>
import Loading from "../components/CityViewSkeleton.vue"
import AsyncCityView from "../components/AsyncCityView.vue";
</script>
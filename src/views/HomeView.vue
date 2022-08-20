<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";
import CityList from "../components/CityList.vue";
import CityListSkeleton from "../components/CityListSkeleton.vue";

const searchInput = ref("");
let setTimeoutRef: number;
const mapboxSearchResults = ref<any[] | null>(null);
const hasError = ref(false);
const mapboxAPIKey =
  "pk.eyJ1Ijoiam9obmtvbWFybmlja2kiLCJhIjoiY2t5NjFzODZvMHJkaDJ1bWx6OGVieGxreSJ9.IpojdT3U3NENknF6_WhR2Q";

const handleInput = () => {
  clearTimeout(setTimeoutRef);
  setTimeoutRef = setTimeout(async () => {
    hasError.value = false;
    if (searchInput.value) {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchInput.value}.json?access_token=${mapboxAPIKey}&types=place`
        );
        mapboxSearchResults.value = result.data.features;
      } catch {
        hasError.value = true;
        mapboxSearchResults.value = null;
      }
    } else {
      mapboxSearchResults.value = null;
    }
  }, 300);
};

const router = useRouter();
const previewCity = (searchResult: any) => {
  const [city, state] = searchResult.place_name.split(",");
  router.push({
    name: "cityView",
    params: { state: state.trim(), city: city },
    query: {
      lat: searchResult.geometry.coordinates[1],
      lng: searchResult.geometry.coordinates[0],
      preview: "show",
    },
  });
};
</script>

<template>
  <main class="container text-white">
    <div class="pt-4 mb-6 relative">
      <input
        v-model="searchInput"
        @input="handleInput"
        type="text"
        placeholder="Search for city or place ..."
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
      <ul
        :class="{
          ' bg-weather-secondary-light p-2':
            (!hasError && mapboxSearchResults?.length === 0) || hasError,
        }"
        v-if="mapboxSearchResults || hasError"
        class="absolute top-[66px] w-full duration-100 shadow-lg"
      >
        <p class="py-2" v-if="hasError">
          Sorry, something went wrong, please try again.
        </p>
        <p class="py-2" v-if="!hasError && mapboxSearchResults?.length === 0">
          No results match your query, try a different term.
        </p>
        <template v-else>
          <li
            v-for="result in mapboxSearchResults"
            :key="(result as any).id"
            class="px-4 py-2 cursor-pointer duration-150 bg-weather-secondary-light"
            @click="previewCity(result)"
          >
            {{(result as any).place_name}}
          </li>
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-4">
      <Suspense>
        <CityList />
        <template #fallback>
          <CityListSkeleton />
        </template>
      </Suspense>
    </div>
  </main>
</template>

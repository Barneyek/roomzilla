<template>
  <div class="w-[calc(100%-10rem)] ml-12 pr-12">
    <div class="my-8">
      <Multiselect
        v-model="filterLaunchLocations.search"
        :options="state.launchLocations"
        placeholder="Filter by launch locations"
        mode="tags"
        class="min-w-32"
      >
        <template v-slot:option="{ option }">
          tu checkbox {{ option.value }}
        </template>
      </Multiselect>
    </div>
    <div
      v-for="(el, index) in filtered"
      :key="index"
      class="flex gap-x-4 w-full py-6 relative after:content-[''] after:bg-[#eaf3f9] after:absolute after:bottom-0 after:w-full after:h-0.5"
    >
      <div class="bg-[#2586bc] rounded-lg p-2">
        <img src="../assets/img/rocket.svg" />
      </div>
      <div class="flex flex-col">
        <p class="font-bold">{{ el.name }}</p>
        <p class="text-sm">{{ el.pad.name }}</p>
      </div>
      {{ moment(el.window_start).format("DD MMM YYYY") }}
      {{ el.pad.location.name }}
      {{ el.status.name }}
    </div>
  </div>
</template>

<script setup>
import Multiselect from "@vueform/multiselect";
import { reactive, computed } from "vue";
import axios from "axios";
import moment from "moment";

const state = reactive({
  launches: [],
  launchLocations: [],
});

const filterLaunchLocations = reactive({
  search: [],
});

const upcomingLaunches = async () => {
  await axios
    .get(`https://space-api.gset.pl/launches?_limit=10&_order=asc`)
    .then((res) => {
      state.launches = res.data;
    });
};

upcomingLaunches();

const getLaunchLocations = async () => {
  await axios.get("https://space-api.gset.pl/locations").then((res) => {
    state.launchLocations = Object.values(res.data).map((el) => el.name);
  });
};

getLaunchLocations();

const filtered = computed(() => {
  if (
    filterLaunchLocations.search === null ||
    filterLaunchLocations.search.length === 0
  ) {
    return state.launches;
  }

  return state.launches.filter((item) =>
    filterLaunchLocations.search.includes(item.pad.location.name)
  );
});
</script>
<style src="@vueform/multiselect/themes/default.css"></style>

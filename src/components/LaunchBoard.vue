<template>
  <div class="w-[calc(100%-10rem)] ml-12 pr-12 font-['Open_Sans']">
    <div class="mt-10 mb-6">
      <Multiselect
        v-model="filterLaunchLocations.search"
        :options="state.launchLocations"
        placeholder="Filter by launch locations"
        :searchable="true"
        mode="tags"
        class="relative custom-multiselect"
        :classes="multiSelectClasses"
      >
        <template v-slot:option="{ option }">
          <input
            type="checkbox"
            class="border-[#2586bc] rounded h-3 w-3 mr-2"
            :id="option.value"
          />
          <!--          <label :for="option.value" class="cursor-pointer">{{ option.value }}</label>-->
          {{ option.value }}
        </template>
      </Multiselect>
    </div>
    <div
      v-for="(el, index) in filtered"
      :key="index"
      class="flex -mx-2 gap-x-4 w-full py-6 relative after:content-[''] after:bg-[#eaf3f9] after:absolute after:bottom-0 after:w-full after:h-0.5"
    >
      <div class="w-5/12 px-2 flex gap-x-4">
        <div>
          <div class="bg-[#2586bc] rounded-lg p-2">
            <img src="../assets/img/rocket.svg" />
          </div>
        </div>
        <div class="flex flex-col">
          <p class="font-bold text-[16px] text-[#2f3031]">{{ el.name }}</p>
          <p class="text-sm text-[#2f3031] leading-7">{{ el.pad.name }}</p>
        </div>
      </div>
      <div class="w-2/12 px-2">
        <div class="flex flex-col">
          <p class="text-sm text-[#2f3031]">
            {{ moment(el.window_start).format("DD MMM YYYY") }}
          </p>
          <p class="text-[13px] text-gray-400 leading-7">Launch date</p>
        </div>
      </div>
      <div class="w-3/12 px-2">
        <div class="flex flex-col">
          <p class="text-sm text-[#2f3031]">{{ el.pad.location.name }}</p>
          <p class="text-[13px] text-gray-400 leading-7">Location</p>
        </div>
      </div>
      <div class="w-2/12 px-2 flex justify-end items-center">
        <p
          class="bg-[#f4f9fb] text-[#2586bc] font-semibold py-1 px-2 text-[13px] rounded cursor-pointer"
        >
          {{ el.status.name }}
        </p>
      </div>
    </div>
    <Pagination />
  </div>
</template>

<script setup>
import Multiselect from "@vueform/multiselect";
import { reactive, computed } from "vue";
import axios from "axios";
import moment from "moment";
import Pagination from "@/components/Pagination.vue";

const multiSelectClasses = {
  container:
    "relative max-w-lg py-1 w-full flex items-center justify-end py-[8px] box-border cursor-pointer bg-[#f4f9fb] rounded text-base leading-snug outline-none",
  containerActive: "",
  tagsSearchWrapper: "inline-block relative mb-1 flex-grow flex-shrink h-full",
  tagsSearch:
    "absolute inset-0 border-0 outline-none focus:ring-0 appearance-none p-0 text-base font-sans box-border w-full bg-transparent text-[#2586bc]",
  tag: "bg-[#2586bc] text-white text-sm font-semibold pl-2 rounded mr-1 mb-1 flex items-center whitespace-nowrap rtl:pl-0 rtl:pr-2 rtl:mr-0 rtl:ml-1",
  tags: "flex-grow flex-shrink flex flex-wrap items-center mt-1 pl-9 rtl:pl-0 rtl:pr-2",
  placeholder:
    "flex items-center h-full absolute left-5 top-0 pointer-events-none bg-transparent text-[14px] leading-snug pl-3.5 text-[#a1cae2] rtl:left-auto rtl:right-0 rtl:pl-0 rtl:pr-3.5",
  caret:
    "multiselect-caret bg-center bg-[#2586bc] bg-no-repeat w-2.5 h-4 py-px box-content mr-3.5 relative z-10 flex-shrink-0 flex-grow-0 transition-transform transform pointer-events-none rtl:mr-0 rtl:ml-3.5",
  caretOpen: "rotate-180 pointer-events-auto",
  clearIcon:
    "multiselect-clear-icon bg-center bg-no-repeat bg-[#2586bc] w-2.5 h-4 py-px box-content inline-block",
  dropdown:
    "multiselect-dropdown max-h-60 absolute -left-px -right-px bottom-0 transform translate-y-full -mt-px overflow-y-scroll border-none z-50 bg-white flex flex-col rounded-b shadow-xl",
  dropdownHidden: "hidden",
  option:
    "flex items-center justify-start box-border text-left cursor-pointer text-base leading-snug py-2 px-3",
};

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

<style src="@vueform/multiselect/themes/default.css" />
<style>
.custom-multiselect:after {
  content: url("../assets/img/magnifier.svg");
  position: absolute;
  margin-top: 1px;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
}
</style>

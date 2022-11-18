<template>
  <div class="relative flex items-center mt-6 mb-10">
    <div class="flex items-center">
      <div
        class="w-6 relative cursor-pointer after:content-['<'] after:text-xl after:text-[#eaf3f9] after:absolute after:left-[10px] after:top-1/2 after:translate-y-[-50%]"
      />
      <div class="flex gap-x-3 pl-2">
        <p class="bg-[#2586bc] rounded text-white px-2 py-0.5 cursor-pointer">
          {{ currentPage + 1 }}
        </p>
        <p
          class="bg-[#f2f2f2] rounded text-[#2f3031] px-2 py-0.5 cursor-pointer"
        >
          {{ nextPage }}
        </p>
        <p
          class="bg-transparent rounded text-[#2f3031] px-1 py-0.5 cursor-pointer"
        >
          {{ totalPages }}
        </p>
      </div>
      <div
        class="w-6 relative cursor-pointer cursor-pointer after:content-['>'] after:text-xl after:text-[#2586bc] after:absolute after:left-[10px] after:top-1/2 after:translate-y-[-50%]"
      />
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "Pagination",
  setup() {
    const totalPages = ref(3);
    const currentPage = ref(0);
    const nextPage = ref(2);

    return {
      totalPages,
      currentPage,
      nextPage,
    };
  },
};
</script>

<template>
  <div class="relative flex items-center justify-center">
    <div class="flex items-center">
      <div
        class="h-8 w-8 bg-[#1f2139] rounded-full relative cursor-pointer after:content-['<'] after:text-xl after:text-white after:absolute after:left-[10px] after:top-1/2 after:translate-y-[-50%]"
      />
      <span class="mr-2 ml-2">{{ currentPage + 1 }} of {{ totalPages() }}</span>
      <div
        class="h-8 w-8 bg-[#1f2139] rounded-full relative cursor-pointer after:content-['>'] after:text-xl after:text-white after:absolute after:left-[10px] after:top-1/2 after:translate-y-[-50%]"
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

    return {
      totalPages,
      currentPage,
    };
  },
};
</script>

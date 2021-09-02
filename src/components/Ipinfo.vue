<template>
  <div
    class="bg-white max-w-xl w-11/12 h-14 absolute top-3/4 md:top-auto md:bottom-0 left-1/2 transform -translate-x-1/2 md:translate-y-1/2 z-20 rounded-md overflow-hidden md:h-auto shadow-2xl"
    :class="{ 'h-auto': showInfo }"
  >
    <div
      v-if="ipData.ip"
      class="flex flex-wrap flex-col md:flex-row text-gray-900 font-sans px-5 py-2 md:py-4 text-left justify-between items-start gap-1 md:gap-2"
    >
      <div>
        <p class="uppercase text-xs md:text-sm font-mono ">ip address</p>
        <h2 class="font-sans font-semibold text-base md:text-lg">
          {{ ipData.ip }}
        </h2>
      </div>
      <div>
        <p class="uppercase text-xs md:text-sm font-mono">location</p>
        <h2 class="font-sans font-semibold text-base md:text-lg capitalize">
          {{ ipData.location }}
        </h2>
      </div>
      <div>
        <p class="uppercase text-xs md:text-sm font-mono">time zone</p>
        <h2 class="font-sans font-semibold text-base md:text-lg uppercase">
          utc {{ ipData.timezone }}
        </h2>
      </div>
      <div>
        <p class="uppercase text-xs md:text-sm font-mono">isp</p>
        <h2 class="font-sans font-semibold text-base md:text-lg capitalize">
          {{ ipData.isp }}
        </h2>
      </div>
    </div>
    <div
      v-else
      class="flex items-center justify-center h-20 font-semibold text-base md:text-lg font-sans"
    >
      <h1>Invalid IP Address.</h1>
    </div>
    <button
      v-show="ipData.ip"
      @click="showInfo = !showInfo"
      class="absolute right-3 top-2 p-2 rounded-tr-md rounded-br-md  md:hidden"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-6 w-6 transform rotate-90 transition-transform"
        :class="{ '-rotate-90': showInfo }"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M9 5l7 7-7 7"
        />
      </svg>
    </button>
  </div>
</template>

<script>
import { ref } from "@vue/reactivity";
export default {
  props: { ipData: Object },
  setup() {
    const showInfo = ref(false);

    return { showInfo };
  },
};
</script>

<style></style>

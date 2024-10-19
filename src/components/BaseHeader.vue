<script setup>
const emit = defineEmits(["openAddActivityModal"]);
const openModal = () => {
  emit("openAddActivityModal");
};
</script>

<template>
  <div class="border flex justify-between items-center py-4 px-8">
    <div class="">ACTIVITY</div>
    <div>
      <span
        @click="openModal"
        class="text-sm whitespace-nowrap cursor-pointer rounded p-2 bg-[#FF5858] text-white flex items-center gap-2 px-4 py-2 transition-all hover:scale-105 hover:bg-opacity-70 focus:scale-105 active:scale-95 active:bg-opacity-80 disabled:pointer-events-none disabled:opacity-40"
        >Add new Activity</span
      >
    </div>
  </div>
</template>

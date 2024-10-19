<script setup>
const activityData = defineModel("activityData");
const emit = defineEmits(["editActivity"]);
const props = defineProps(["isEdit"]);

const handleToggleChange = (event, index) => {
  console.log(event, "This is Event");
  console.log(index, "This is index");
};
const handleEditActivity = (index) => {
  emit("editActivity", index);
};
const handleDeleteActivity = (index) => {
  activityData.value.splice(index, 1);
  localStorage.setItem("activities", JSON.stringify(activityData.value));
};
</script>

<template>
  <div class="grid grid-cols-2 gap-4">
    <div
      v-for="(activity, index) in activityData"
      :key="index"
      class="border rounded col-span-1 p-4 mt-2"
    >
      <div class="flex items-center justify-between">
        <div class="flex flex-col">
          <div>
            <p class="text-xs text-gray-400">Activity Name</p>
            <p>{{ activity?.activityName }}</p>
          </div>
          <div class="mt-2">
            <p class="text-xs text-gray-400">Status</p>
            <label class="inline-flex mt-2 items-center cursor-pointer">
              <input
                type="checkbox"
                v-model="activity.status"
                value=""
                class="sr-only peer"
                @change="handleToggleChange($event, index)"
              />
              <div
                class="relative w-11 h-6 bg-gray-200 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"
              ></div>
            </label>
          </div>
        </div>
        <div class="flex gap-4">
          <img
            src="/src/assets/edit-gray.svg"
            class="cursor-pointer"
            alt="Edit Icon"
            @click="handleEditActivity(index)"
          />
          <img
            src="/src/assets/delete-gray.svg"
            class="cursor-pointer"
            alt="Delete Icon"
            @click="handleDeleteActivity(index)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

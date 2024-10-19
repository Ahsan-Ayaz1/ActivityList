<script setup>
import { ref, onMounted, watch, computed } from "vue";
import BaseHeader from "./BaseHeader.vue";
import BaseModal from "./BaseModal.vue";
import ActivityCard from "./ActivityCard.vue";

const isModalOpen = ref(false);
const activityData = ref([]);
const activityDetails = ref({});
const activityIndex = ref(null);
const isEdit = ref(false);
const closeModal = () => {
  isModalOpen.value = false;
  activityDetails.value = {};
};
const openActivityModal = () => {
  isModalOpen.value = true;
};
const saveToLocalStorage = () => {
  localStorage.setItem("activities", JSON.stringify(activityData.value));
};
const manageActivity = () => {
  if (!isEdit.value) {
    activityData.value.push(activityDetails.value);
    activityDetails.value = {};
    isModalOpen.value = false;
    saveToLocalStorage();
  } else {
    activityData.value[activityIndex.value] = activityDetails.value;
    isModalOpen.value = false;
    isEdit.value = false;
    activityDetails.value = {};
  }
};
const editActivity = (index) => {
  isEdit.value = true;
  isModalOpen.value = true;
  activityDetails.value = activityData.value[index];
  activityIndex.value = index;
};
// Conputed Property To Disable Button if ActivityName is Empty
const isSaveDisabled = computed(() => !activityDetails.value.activityName?.trim());

watch(activityData, saveToLocalStorage, { deep: true });

onMounted(() => {
  const storedActivities = localStorage.getItem("activities");
  if (storedActivities) {
    activityData.value = JSON.parse(storedActivities);
  }
});
</script>

<template>
  <div>
    <BaseHeader @openAddActivityModal="openActivityModal" />
    <div class="mt-10 px-8">
      <div v-if="activityData?.length">
        <h1 class="text-2xl font-medium">Activity List</h1>
      </div>
      <div class="">
        <ActivityCard
          v-model:activityData="activityData"
          :is-edit="isEdit"
          @editActivity="editActivity"
        />
      </div>
    </div>
    <BaseModal
      :custom-class="'w-[400px]'"
      :title="isEdit ? 'Update Activity' : 'Add Activity'"
      :is-visible="isModalOpen"
      @close="closeModal"
    >
      <!-- Body Content -->
      <div class="flex flex-col">
        <label for="activityName" class="text-sm mb-1">Activity Name</label>
        <input
          type="text"
          id="activityName"
          v-model="activityDetails.activityName"
          class="border outline-none border-gray-300 rounded p-2 text-sm"
        />
      </div>
      <div class="text-sm mt-2">
        <label for="activityStatus" class="text-sm">Activity Status</label>
        <select
          name="activityStatus"
          v-model="activityDetails.status"
          id="activityStatus"
          class="border w-full p-2 outline-none mt-1 border-gray-300 rounded"
        >
          <option value="true">Completed</option>
          <option value="false">Not Completed</option>
        </select>
      </div>
      <!-- Footer Buttons -->
      <template #footer>
        <div class="p-4 items-center flex gap-4">
          <button
            @click="manageActivity"
            :disabled="isSaveDisabled"
            class="text-sm whitespace-nowrap cursor-pointer rounded p-2 bg-[#FF5858] text-white flex items-center gap-2 px-4 py-2 transition-all hover:scale-105 hover:bg-opacity-70 focus:scale-105 active:scale-95 active:bg-opacity-80 disabled:pointer-events-none disabled:opacity-40"
          >
            {{ isEdit ? "Update Activity" : "Add Activity" }}
          </button>
          <button
            class="text-sm whitespace-nowrap cursor-pointer rounded p-2 border text-gray-500 flex items-center gap-2 px-4 py-2 transition-all hover:scale-105 hover:bg-opacity-70 focus:scale-105 active:scale-95 active:bg-opacity-80 disabled:pointer-events-none disabled:opacity-40"
            @click="closeModal"
          >
            Cancel
          </button>
        </div>
      </template>
    </BaseModal>
  </div>
</template>

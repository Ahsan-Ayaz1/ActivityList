<template>
  <Transition name="slide-from-right">
    <div
      class="fixed inset-0 z-50 flex justify-end h-screen duration-500 bg-black bg-opacity-20"
      v-if="isVisible"
      @click="closeModalOnOverlayClick"
    >
      <div class="bg-white border flex flex-col" :class="customClass">
        <template v-if="title">
          <!-- Header -->
          <div class="flex items-center justify-between px-4 py-2">
            <div class="flex items-center">
              <h2 class="text-2xl font-semibold">{{ title }}</h2>
              <p class="mt-1 ml-1 text-xs" v-if="description">
                {{ description }}
              </p>
            </div>
            <img
              class="cursor-pointer"
              @click="closeModal"
              src="/src/assets/cross-icon.svg"
              alt="cross"
            />
          </div>
          <hr />
        </template>

        <!-- Body with optional scrolling -->
        <div class="px-4 overflow-auto flex-1 mr-1 my-2">
          <slot></slot>
        </div>

        <!-- Footer -->
        <div class="bg-white border-t">
          <slot name="footer"></slot>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script setup>
defineProps({
  isVisible: {
    type: Boolean,
    default: false,
  },
  customClass: {
    type: String,
    default: "",
  },
  title: {
    type: String,
    default: "",
  },
  description: {
    type: String,
    default: null,
  },
  isScrollable: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits(["close"]);

const closeModal = () => {
  emit("close");
};

const closeModalOnOverlayClick = (event) => {
  if (event.target === event.currentTarget) {
    closeModal();
  }
};
</script>

<style>
.slide-from-right-enter-active,
.slide-from-right-leave-active {
  transition: transform 0.7s ease, opacity 0.5s ease;
  opacity: 1;
}

.slide-from-right-enter,
.slide-from-right-leave-to {
  transform: translateX(100%);
  opacity: 0;
}

.slide-from-right-enter-to,
.slide-from-right-leave {
  transform: translateX(0%);
  opacity: 1;
}

.fade-enter-active,
.fade-leave-active {
  transition-duration: 0.3s;
  transition-property: opacity;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>

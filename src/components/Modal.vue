<script setup lang="ts">
import { onMounted, ref } from "vue";

const props = withDefaults(
  defineProps<{
    show?: boolean;
    title?: string;
    message?: string;
  }>(),
  {
    show: false,
    title: "Title",
    message: "Some action",
  }
);
const emit = defineEmits<{
  (e: "close"): void;
}>();

const modal = ref<HTMLElement | null>(null);

onMounted(() => {
  modal.value?.focus();
});
</script>
<template>
  <teleport to="#modal">
    <transition name="modal">
      <div
        v-if="show"
        ref="modal"
        tabindex="-1"
        class="fixed z-30 inset-0 flex items-center justify-center"
        @keydown.esc="emit('close')"
      >
        <div
          @click="emit('close')"
          class="z-20 absolute bg-white opacity-10 w-full h-full"
        ></div>
        <div class="z-30 w-1/2 bg-white flex flex-col rounded">
          <slot></slot>
        </div>
      </div>
    </transition>
  </teleport>
</template>

<style scoped>
.modalButton {
  padding-top: 1rem;
  padding-bottom: 1rem;
  padding-left: 3rem;
  padding-right: 3rem;
}

.modal-enter-active,
.modal-leave-active {
  transition: all 0.4s ease;
}
.modal-enter-from,
.modal-leave-to {
  opacity: 0;
  transform: scale(1.1);
}
</style>

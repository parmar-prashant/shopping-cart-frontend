<template>
  <div class="modal">
    <div class="relative modal-content w-5/6 sm:w-8/12 md:w-3/4 lg:w-1/2 h-3/4 sm:h-8/12 md:h-3/4 lg:h-3/4">
      <div class="absolute -top-3 -right-3 text-gray-100 cursor-pointer" @click="$emit('closeModal')">
        <svg xmlns="http://www.w3.org/2000/svg" fill="black" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
          class="w-10 h-10">
          <path stroke-linecap="round" stroke-linejoin="round"
            d="m9.75 9.75 4.5 4.5m0-4.5-4.5 4.5M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
        </svg>
      </div>
      <slot></slot>
    </div>
  </div>
</template>
  
<script>
export default {
  name: 'Modal',
  emits: ['closeModal'],
};
</script>
  
<style>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 10px;
  border-radius: 8px;
  width: 50%;
  height: 75%;
  max-height: 75%;
  overflow-x: hidden;
  overflow-y: auto;
}
</style>
  
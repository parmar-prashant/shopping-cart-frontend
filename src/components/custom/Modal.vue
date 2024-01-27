<template>
  <div class="modal">
    <div class="relative modal-content w-5/6 sm:w-8/12 md:w-3/4 lg:w-1/2 h-3/4 sm:h-8/12 md:h-3/4 lg:h-3/4">
      <div class="flex justify-start sm:justify-end">
        <button class="rounded-md bg-gray-200 p-2" @click="$emit('closeModal')">Close</button>
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
  
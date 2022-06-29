<template>
  <div
    class="w-full flex items-center justify-between p-0.5 bg-white rounded my-2"
  >
    <input
      type="text"
      class="outline-none p-0.5 px-2"
      v-model="item.data_title"
    />
    <div class="flex gap-2 items-center">
      <button class="w-3 h-3 rounded-full bg-red-600"></button>
      <button class="text-lg px-1" @click="$emit('delete', item.id)">
        <i class="bi bi-x"></i>
      </button>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    item: {
      type: Object,
      default: () => {},
    },
  },
};
</script>

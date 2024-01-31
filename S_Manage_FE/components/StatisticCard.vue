<script setup>
import { defineProps } from 'vue';

defineProps({
  Data: {
    type: Object,
    required: true,
  },
});

// defineEmits(['clickShowPopup', 'clickCloseAllPopup', 'clickOnDelete', 'clickEditUser']);
</script>

<template>
  <div>
    <div
      :style="{ backgroundColor: Data.color }"
      class="item-user flex flex-col items-center m-6 p-4 justify-evenly border-solid border-2 rounded-md w-40 h-40">
      <h3 class="font-bold text-lg">{{ Data.title }}</h3>
      <p class="font-medium text-base">{{ Data.data }}</p>
    </div>

    <!-- <div @click="$emit('clickCloseAllPopup')" class="hidden overlay absolute top-0 right-0 left-0 bottom-0"></div> -->
  </div>
</template>

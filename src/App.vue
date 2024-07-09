<template>
  <div class="container mx-auto p-4">
    <button @click="openRatingPopup" class="bg-blue-500 text-white p-2 rounded-md hover:bg-blue-600">Rate the Product</button>

    <div v-if="showRatingPopup" class="fixed inset-0 flex items-center justify-center bg-gray-800 bg-opacity-75">
      <RatingComponent @close-popup="closeRatingPopup" />
    </div>
  </div>
</template>

<script>
import RatingComponent from './components/RatingComponent.vue';

export default {
  components: {
    RatingComponent,
  },
  data() {
    return {
      showRatingPopup: false,
    };
  },
  methods: {
    openRatingPopup() {
      this.showRatingPopup = true;
    },
    closeRatingPopup() {
      this.showRatingPopup = false;
    },
  },
};
</script>

<style scoped>
.container {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>

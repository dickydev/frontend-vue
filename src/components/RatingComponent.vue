<template>
  <div class="max-w-md mx-auto p-4">
    <div class="bg-white rounded-lg shadow-md p-6">
      <div class="text-lg font-semibold mb-2">How would you rate your satisfaction with our product?</div>
      <div class="flex flex-col items-center">
        <div class="flex space-x-1 mb-2">
          <span
            v-for="star in 5"
            :key="star"
            @click="rate(star)"
            @mouseover="hover(star)"
            @mouseleave="resetHover"
            class="cursor-pointer text-gray-400 hover:text-gray-600 star"
            :class="{
              'text-gray-600': rating >= star || hoverRating >= star,
              'text-yellow-400': rating >= star || hoverRating >= star, 
            }"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 mr-3" viewBox="0 0 20 20" fill="currentColor">
              <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.518 4.674a1 1 0 00.95.69h4.917c.969 0 1.371 1.24.588 1.81l-3.977 2.89a1 1 0 00-.364 1.118l1.518 4.674c.3.921-.755 1.688-1.538 1.118l-3.977-2.89a1 1 0 00-1.175 0l-3.977 2.89c-.783.57-1.838-.197-1.538-1.118l1.518-4.674a1 1 0 00-.364-1.118L2.593 9.1c-.783-.57-.381-1.81.588-1.81h4.917a1 1 0 00.95-.69l1.518-4.674z"/>
            </svg>
            <span class="text-center text-gray-600 text-sm block mr-3">{{ star }}</span>
          </span>
        </div>
        <div class="flex justify-around w-full">
          <span class="text-gray-600 text-sm">Very Dissatisfied</span>
          <span class="text-gray-600 text-sm">Very Satisfied</span>
        </div>
      </div>
      <textarea
        v-model="comment"
        placeholder="Leave a comment (optional)"
        class="mt-4 w-full p-2 border border-gray-300 rounded-md"
      ></textarea>
      <button
        @click="submitFeedback"
        class="mt-4 w-full bg-blue-500 text-white p-2 rounded-md hover:bg-blue-600"
      >Submit</button>

      <!-- Pop-up -->
      <div v-if="showPopup" class="fixed inset-0 flex items-center justify-center bg-gray-800 bg-opacity-75">
        <div class="bg-white rounded-lg shadow-md p-6">
          <h2 class="text-lg font-semibold mb-2">Feedback Submitted</h2>
          <p class="text-gray-600 mb-4">Thank you for your feedback!</p>
          <button @click="closePopup" class="bg-blue-500 text-white p-2 rounded-md hover:bg-blue-600">Close</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      rating: 0,
      hoverRating: 0,
      comment: '',
      showPopup: false, // Add showPopup data property
    };
  },
  methods: {
    rate(star) {
      this.rating = star;
    },
    hover(star) {
      this.hoverRating = star;
    },
    resetHover() {
      this.hoverRating = 0;
    },
    async submitFeedback() {
      if (this.rating === 0) {
        alert('Please provide a rating');
        return;
      }

      const feedback = {
        score: this.rating,
        comment: this.comment,
        created_at: new Date().toISOString(),
      };

      try {
        await axios.post('http://localhost:8000/feedback/', feedback);
        this.showPopup = true;
        setTimeout(() => {
          location.reload();
        }, 800); // Refresh page after 2 seconds
      } catch (error) {
        alert('Failed to submit feedback');
      }
    },
    openPopup() {
      this.showPopup = true;
    },
    closePopup() {
      this.showPopup = false;
    },
  },
};
</script>

<style scoped>
.grad-text {
  background: -webkit-linear-gradient(45deg, #f3ec78, #af4261);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.star {
  transition: transform 0.3s ease-in-out;
}

.star:hover {
  transform: scale(1.2);
}
</style>

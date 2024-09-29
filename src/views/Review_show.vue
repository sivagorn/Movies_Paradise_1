<template>
  <div class="container">
    <div v-if="isLoading" class="loading">Loading...</div>
    <div v-else class="grid">
      <div class="card" v-for="review in sortedReviews" :key="review.index">
        <div class="card-content">
          <p class="name">{{ review.name }}</p>
          <br>
          <p class="movie-title">{{ review.movie_name }}</p>
          <br>
          <p class="movie-rating">{{ review.rating }}</p>
          <br>
          <p class="movie-comment">{{ review.comment }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, reactive, computed } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

const router = useRouter();

const state = reactive({
  reviews: [],
  isLoading: true 
});

const sortedReviews = computed(() => {
  return state.reviews.slice().reverse(); 
});

const fetchReviews = async () => {
  const url = 'https://script.google.com/macros/s/AKfycbzABRWHUuJyCth8H-Ri9-aSwEHa3SX8-Kvs8Oop6miNuZpV3awOTd0H5yZuwopu3Bca/exec';
  
  try {
    const response = await axios.get(url);
    console.log(response.data); 
    state.reviews = response.data; 
    localStorage.setItem('reviews', JSON.stringify(state.reviews)); 
  } catch (error) {
    console.error('Error fetching reviews:', error);
  } finally {
    state.isLoading = false; 
  }
};

onMounted(fetchReviews);
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Itim&display=swap');


.container {
  font-family: 'Itim', cursive; 
  margin-top: 50px;
  padding-left: 70px;
  padding-right: 70px; 
}

.grid {
  display: grid; 
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); 
  gap: 3rem; 
}

.card {
  display: flex;
  flex-direction: column;
  border: 5px solid #000000;
  border-radius: 5px; 
  box-sizing: border-box;
  background-color: #ffffff;
  transition: transform 0.3s; 
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  height: 100%; 
}

.card:hover {
  transform: translateY(-10px); 
}

.card-content {
  padding: 1rem;
  text-align: center;
  flex-grow: 1; 
}

.movie-title {
  font-size: 22px;
  margin: 0;
  color: rgb(0, 0, 0);
}

.movie-rating {
  font-size: 22px;
  margin: 0;
  color: rgb(0, 0, 0);
}

.movie-comment {
  font-size: 20px;
  margin: 0;
  color: rgb(0, 0, 0);
}

.loading {
  text-align: center;
  font-size: 20px;
  color: #555;
}

.name {
  font-size: 25px;
  margin: 0;
  color: rgb(0, 0, 0);
}


@media (max-width: 768px) {
  .grid {
    display: flex; 
    flex-wrap: wrap; 
    justify-content: center; 
  }

  .card {
    flex: 1 1 260px; 
    margin: 5px; 
    height: 300px; 
  }
}
</style>

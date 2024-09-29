<template>
  <AppLayout>
    <div class="container">
      <div v-if="loading" class="loader">Loading...</div>
      <div v-else class="movie-grid">
        <div 
          v-for="movie in state.movies" 
          :key="movie.movie_id" 
          class="card" 
          @click="goToDetail(movie.movie_id)"
        >
          <img 
            class="poster" 
            :src="movie.poster" 
            alt="Poster"
          />
          <div class="card-content">
            <p class="movie-title">{{ movie.title }}</p>
          </div>
        </div>
      </div>
    </div>
  </AppLayout>
</template>

<script setup>
import { onMounted, reactive, ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

const router = useRouter();

const state = reactive({
  movies: [] 
});

const loading = ref(true);

const goToDetail = (movie_id) => {
  router.push({ name: 'MovieDetail', params: { index: movie_id } }); 
}

onMounted(() => {
  const cachedMovies = localStorage.getItem('movies');
  if (cachedMovies) {
    state.movies = JSON.parse(cachedMovies);
    loading.value = false; 
  } else {
    const url = 'https://script.googleusercontent.com/macros/echo?user_content_key=6Zaipk-Qwe6VZ7ZqU8w2ZU90rICEwPtRs_EN3rbBjYdiOdHts-WyF0FLutmgMvKXLfk05TWB6dwWOc15I_EyID6t_lap3DAgm5_BxDlH2jW0nuo2oDemN9CCS2h10ox_1xSncGQajx_ryfhECjZEnL8u_AcB_1UHpYPCdf8vF9NY5MH5fDAIIq05qmQpVHYvIVZediGCqp9VPBpAUZwcVLrWvMx9lReKudZEoNYGr_Nl257qV3UnLQ&lib=MlS1jc-5j_78AMWoObAbgVkPwlNntp4vT';
    axios.get(url)
      .then((response) => {
        state.movies = response.data; 
        localStorage.setItem('movies', JSON.stringify(state.movies));
      })
      .finally(() => {
        loading.value = false; 
      });
  }
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Itim&display=swap'); 

.container {
  margin-top: 50px;
  margin-bottom: 70px;
}

.movie-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr); 
  gap: 2rem; 
}

.card {
  display: flex;
  flex-direction: column;
  border: 5px solid #000000;
  box-sizing: border-box;
  background-color: #ffffff;
  transition: transform 0.3s ease, box-shadow 0.3s ease; 
}

.card:hover {
  transform: translateY(-10px); 
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2); 
}

.poster {
  width: 100%;
  height: auto; 
  object-fit: cover;
}

.card-content {
  padding: 0.5rem; 
  text-align: center;
}

.movie-title {
  padding-top: 15px;
  padding-bottom: 15px;
  padding-left: 5px;
  padding-right: 5px;
  font-family: "Itim", sans-serif; 
  font-weight: normal; 
  font-size: 24px; 
  margin: 0;
  color: rgb(0, 0, 0);
  text-align: center; 
}

.loader {
  text-align: center;
  font-size: 24px;
  color: #000;
}

@media (max-width: 480px) {
  .movie-grid {
    display: flex; 
    flex-direction: column; 
    align-items: center; 
    justify-content: center; 
  }

  .movie-grid {
    grid-template-columns: repeat(1, 1fr); 
  }

  .card {
    max-width: 300px; 
    margin: 0 auto; 
  }

  .movie-title {
    font-size: 22px; 
  }

  .card-content {
    padding: 0.5rem; 
    text-align: center;
  }
}
</style>

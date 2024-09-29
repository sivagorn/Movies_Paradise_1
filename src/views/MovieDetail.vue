<script setup>
import axios from 'axios';
import { onMounted, reactive, ref } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const state = reactive({
  movies: [],
  movie: null,
});
const loading = ref(true);
const error = ref(null); 

const fetchMovies = async () => {
  const cachedMovies = localStorage.getItem('movies');
  
  if (cachedMovies) {
    state.movies = JSON.parse(cachedMovies);
  } else {
    const url = 'https://script.googleusercontent.com/macros/echo?user_content_key=6Zaipk-Qwe6VZ7ZqU8w2ZU90rICEwPtRs_EN3rbBjYdiOdHts-WyF0FLutmgMvKXLfk05TWB6dwWOc15I_EyID6t_lap3DAgm5_BxDlH2jW0nuo2oDemN9CCS2h10ox_1xSncGQajx_ryfhECjZEnL8u_AcB_1UHpYPCdf8vF9NY5MH5fDAIIq05qmQpVHYvIVZediGCqp9VPBpAUZwcVLrWvMx9lReKudZEoNYGr_Nl257qV3UnLQ&lib=MlS1jc-5j_78AMWoObAbgVkPwlNntp4vT';
    
    try {
      const response = await axios.get(url);
      state.movies = response.data;
      localStorage.setItem('movies', JSON.stringify(state.movies)); 
    } catch (err) {
      error.value = 'Error fetching movies'; 
      console.error('Error fetching movies:', err);
    }
  }

  const movieId = parseInt(route.params.index); 
  state.movie = state.movies.find(movie => movie.movie_id === movieId);
  loading.value = false; 
};

onMounted(fetchMovies);

const getYoutubeId = (url) => {
  const regExp = /(?:youtube\.com\/(?:[^\/]+\/[^\/]+\/|(?:v|e(?:mbed)?)\/|.*[?&]v=)|youtu\.be\/)([^"&?\/ ]{11})/;
  const match = url.match(regExp);
  return match ? match[1] : null;
};
</script>

<template>
  <AppLayout>
    <div v-if="loading" class="loading">Loading...</div>
    <div v-else-if="error" class="error">{{ error }}</div>
    <div v-else-if="state.movie" class="movie-card-container">
      <img :src="state.movie.banner" alt="Banner" class="banner-image">
      <div class="movie-card">
        <img :src="state.movie.poster" alt="Poster">
        <div class="card-content">
          <h2>{{ state.movie.title }}</h2>
          <p>
            <b>ตัวอย่างภาพยนตร์</b><br>
            <div class="video-container">
              <iframe 
                v-if="state.movie.trailer" 
                :src="`https://www.youtube.com/embed/${getYoutubeId(state.movie.trailer)}`" 
                title="YouTube video player" 
                frameborder="0" 
                allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
                allowfullscreen>
              </iframe>
            </div>
            <br><br>
            <b>เรื่องย่อ</b><br>
            {{ state.movie.synopsis }}
            <br><br>
            <b>ประเภท</b><br>
            {{ state.movie.genres }}
            <br><br>
            <b>ความยาว</b><br>
            {{ state.movie.duration }}
            <br><br>
            <b>ผู้กำกับ</b><br>
            {{ state.movie.director }}
            <br><br>
            <b>นักแสดง</b><br>
            {{ state.movie.cast }}
            <br><br>
            <b>สถานะ</b><br>
            -{{ state.movie.status }}-
            <br>
          </p>
        </div>
      </div>
    </div>
  </AppLayout>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Itim&display=swap');

.loading {
  text-align: center;
  font-size: 24px;
  margin-top: 50px;
}

.error {
  color: red;
  text-align: center;
  font-size: 20px;
  margin-top: 50px;
}

.movie-card-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
}

.banner-image { 
  position: fixed; 
  top: 50%; 
  left: 50%; 
  width: 100%; 
  height: 100%; 
  object-fit: cover; 
  object-position: center; 
  z-index: 1; 
  transform: translate(-50%, -50%); 
}

.movie-card {
  position: relative;
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 20px;
  padding: 20px;
  width: 80%; 
  max-width: 400px; 
  z-index: 2;
  margin: auto; 
}

.video-container {
  position: relative;
  padding-bottom: 56.25%; 
  height: 0;
  overflow: hidden;
  max-width: 100%; 
}

.video-container iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}


@media (max-width: 767px) {
  .card-content {
    font-size: 10px; 
  }

  .card-content h2 {
    font-size: 20px; 
  }

  .card-content p {
    font-size: 15px; 
  }
}

@media (min-width: 768px) {
  .movie-card {
    max-width: 700px; 
  }
}

@media (min-width: 1024px) {
  .movie-card {
    max-width: 900px; 
  }
  .card-content h2 {
  font-size: 30px;
  margin-bottom: 10px;
}

.card-content p {
  font-size: 20px;
}
}

.movie-card img {
  max-width: 38%;
  border-radius: 20px;
}

.card-content {
  font-family: "Itim", sans-serif; 
  margin-left: 20px;
  margin-top: 30px;
}

</style>


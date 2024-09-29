<script setup>
import { RouterLink, RouterView, useRoute } from 'vue-router';
import { useRouter } from 'vue-router';

const route = useRoute(); 
const router = useRouter(); 

const goBack = () => {
  router.go(-1); 
};
</script>

<template>
  <nav v-if="route.path !== '/' " class="navbar navbar-expand-lg navbar-light">
    <div class="navbar-brand">
      <RouterLink to="/home">
        <img src="/logo.png" alt="VD Movies Logo" class="logo">
      </RouterLink>
      <button class="btn btn-secondary back-button" @click="goBack">ย้อนกลับ</button>
    </div>

    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    
    <div class="collapse navbar-collapse" id="navbarNavDropdown">
      <ul class="navbar-nav mx-auto">
        <li class="nav-item">
          <RouterLink class="btn btn-success other-button" to="/reviews">รีวิวภาพยนตร์ 💡</RouterLink>
        </li>
        <li class="nav-item">
          <RouterLink class="btn btn-warning other-button" to="/review_show">รีวิวอื่นๆ 📽</RouterLink>
        </li>
        <li class="nav-item">
          <RouterLink class="btn btn-danger other-button" to="/reports">แจ้งปัญหา 📢</RouterLink>
        </li>
      </ul>
    </div>
  </nav>

  <RouterView />
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Itim&display=swap');

body {
  background-image: url('/body.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed;
  margin: 0;
  padding: 0;
}

nav {
  background-color: rgb(255, 255, 255);
  position: relative;
  z-index: 2;
  border: 5px solid black;
  border-radius: 10px;
}

.navbar {
  font-family: "Anton", sans-serif;
  font-style: normal;
  font-size: 25px;
}

.navbar-nav {
  display: flex; 
  justify-content: space-between; 
  width: 55%; 
}

.navbar-nav .nav-item:last-child {
  margin-right: 0; 
}

/* CSS สำหรับปุ่ม */
.back-button {
  font-family: 'Itim', cursive; 
  font-size: 15px; 
  padding: 10px 15px; 
  border: 5px solid gray; 
  border-radius: 5px; 
}

.other-button { 
  font-family: 'Itim', cursive; 
  font-size: 24px; 
  padding: 15px 20px; 
  border: 5px solid; 
  border-radius: 5px; 
  margin: 5px;
}

.navbar-nav .btn-success {
  border-color: green; 
}

.navbar-nav .btn-warning {
  border-color: orange; 
}

.navbar-nav .btn-danger {
  border-color: red; 
}


.navbar-brand {
  display: flex; 
  flex-direction: column; 
  align-items: center; 
}

.logo {
  width: 100px; 
  height: auto; 
  margin-left: 10px;
}


.back-button {
  display: none; 
}


@media (max-width: 768px) {
  .back-button {
    display: block; 
    width: 100%; 
    margin-left: 10px;
  }

  .navbar-nav {
    flex-direction: column; 
    align-items: center; 
  }

  .navbar-nav .nav-item {
    margin: 10px 0; 
  }
  
  .logo {
    margin-bottom: 10px;
  }
}
</style>

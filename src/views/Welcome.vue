<template>  
  <div class="welcome-container" :class="{ fade: isFading }">
    <img src="/logoWelcome.png" alt="Logo" class="logo" />
    <h1>ยินดีต้อนรับสู่เว็บไซต์ Movies Paradise</h1>
    <button class="enter-button" @click="navigateToHome">เข้าสู่เว็บไซต์</button>
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router';
import { ref } from 'vue';

const router = useRouter();
const isFading = ref(false); 

const navigateToHome = () => {
  isFading.value = true; 
  setTimeout(() => {
    router.push('/home'); 
  }, 500); 
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Itim&display=swap');

html, body {
  height: 100%; 
  margin: 0; 
}

.welcome-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  text-align: center;
  background-color: black; 
  color: white; 
  font-family: "Itim", cursive; 
  transition: opacity 0.5s ease; 
}

.fade {
  opacity: 0; 
}

.logo {
  width: 80%; 
  max-width: 500px; 
  height: auto; 
  margin-bottom: 20px; 
}

.enter-button {
  margin-top: 20px; 
  padding: 10px 20px;
  font-size: 20px;
  cursor: pointer;
  background-color: #fcd444;
  color: rgb(0, 0, 0);
  border: none;
  border-radius: 5px;
  transition: background-color 0.3s;
  width: 90%; 
  max-width: 300px; 
  font-family: "Itim", cursive; 
}

.enter-button:hover {
  background-color: #fa9494;
}

@media (max-width: 768px) {
  h1 {
    font-size: 24px; 
    font-family: "Itim", cursive; 
  }

  .enter-button {
    font-size: 18px; 
  }
}

@keyframes scale {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
  }
}
</style>

<script>
import ApiGitHub from './components/ApiGitHub.vue';

export default {
  components: {
    ApiGitHub,
  },
  data() {
    return {
      showScrollTop: false,
    };
  },
  methods: {
    checkScroll() {
      // Se la pagina e' stata scrollata di 200px allora:
      this.showScrollTop = window.scrollY > 200;
      //Log "true" di showScrollTop
    },
    scrollToTop() {
      // Funzione per far scrollare la pagina verso l'alto
      window.scrollTo({
        top: 0,
        behavior: 'smooth', //Scroll top lentamente 'smooth' invece di essere istantaneo
      });
    },
  },
  mounted() {
    window.addEventListener('scroll', this.checkScroll);
  },
  beforeDestroy() {
    // Memory leaks
    window.removeEventListener('scroll', this.checkScroll);
  },
}

</script>

<template>
  <div class="page-background"></div>

  <ApiGitHub />

  <!-- Freccia che compare quando scrolli di 200px -->
  <div v-if="showScrollTop" @click="scrollToTop" class="scroll-top-arrow show">
    <i class="fa-solid fa-arrow-up"></i>
  </div>
</template>

<style lang="scss">
@use './style/style.scss' as *;

//Background
.page-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  background: linear-gradient(to right, #b2a8fc, #6aaee2);
}

//Stili per la freccia
.scroll-top-arrow {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: rgba(0, 0, 0, 0.6);
  color: white;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.3s;
}

//Hover Freccia
.scroll-top-arrow:hover {
  background-color: rgba(0, 0, 0, 0.8);
  transform: scale(1.1);
}
</style>

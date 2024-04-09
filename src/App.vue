<script>
import ApiGitHub from './components/ApiGitHub.vue';
import AppHeader from './components/AppHeader.vue';


export default {
  components: {
    ApiGitHub,
    AppHeader,
  },
  data() {
    return {

    };
  },
}

</script>

<template>
  <AppHeader />
  <ApiGitHub />
</template>

<style lang="scss">
@use './style/style.scss' as *;
</style>

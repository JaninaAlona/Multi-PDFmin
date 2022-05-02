<template>
<Menu />
</template>

<script>
import Menu from './components/Menu.vue'

export default {
  name: 'App',
  components: {
    Menu,
  }
}
</script>

<style>

/*https://fonts.google.com/specimen/Radio+Canada#standard-styles*/
@import url('https://fonts.googleapis.com/css2?family=Radio+Canada:wdth,wght@81.5,300;81.5,392;81.5,573;100,300;100,400&display=swap');

#app {
  font-family: 'Radio Canada', sans-serif;
  font-style: "Regular";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>

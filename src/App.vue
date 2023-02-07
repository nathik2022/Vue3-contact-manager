<template>
  <NavBar/>
  <router-view/>
</template>

<style>

</style>

<script>
import NavBar from './components/NavBar.vue';


export default {
  components: { NavBar},
  name: "App",
}
</script>


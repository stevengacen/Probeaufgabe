<template>
  <div class='home'>
    <Gameboard />
  </div>
</template>
  
<script>
  import Gameboard from '@/components/Gameboard.vue';

  export default {
    name: 'Home',
    components: {
      Gameboard
    }
  };
</script>

<style>
  .home {
    position: relative;
    width: 100%;
    height: 100%;
  }
</style>
<template>
  <div id="app">
    <div class="container">
      <Sidebar />
      <div class="content">
        <router-view/>
      </div>
    </div>
    <Footer />
  </div>
</template>

<script>
  import Sidebar from './components/Sidebar.vue';
  import Footer from './components/Footer.vue';


  export default {
    name: 'App',
    components: {
      Sidebar,
      Footer
    },
  };
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');

  .container {
    display: flex;
    height: 100vh;
    background-color: #f0f0f0;
  }

  .content {
    flex-grow: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
  }

  body, html, #app {
    margin: 0;
    padding: 0;
    height: 100%;
    overflow: hidden;
    font-family: "Roboto", "sans-serif";
    font-weight: 100;
    font-style: normal;
  }

  /* Mobile Styles */
@media (max-width: 768px) {

  .footer {
    flex-direction: column;
    text-align: center;
  }

  .content {
    padding: 10px;
  }
}

</style>

<template>
  <div class="header_container">
    <b-nav tabs>
      <b-navbar-brand>
        <img class="logo" src="../assets/thinking.svg" alt="">
      </b-navbar-brand>
      <!-- <b-nav-item disabled>Counter: {{numCorrect}}/{{numTotal}}</b-nav-item> -->
    </b-nav>
  </div>
</template>

<script>
  export default{
    props: [
    'numCorrect',
    'numTotal',
  ],
  }
</script>

<style scoped>
  .header_container{
    background-color: #263056;
    border-bottom: 1px solid rgb(216, 216, 216);
    color: black;
    height: 60px;
    width: 100%;
    position: fixed;
    left: 0;
    top: 0;
    z-index: 5;
    padding: 0 20px;
    display: flex;
    align-items: center;
  }
  .nav-tabs{
    border: none;
  }
  .nav-tabs .nav-link.disabled{
    color: black;
  }
  .logo {
    max-height: 40px;
  }
</style>

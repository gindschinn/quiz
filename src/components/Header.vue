<template>
  <div class="header_container">
    <b-nav tabs>
      <b-navbar-brand><b>Ginas Trivia</b></b-navbar-brand>
      <b-nav-item disabled>Counter: {{numCorrect}}/{{numTotal}}</b-nav-item>
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
    background-color: lightcoral;
    color: black;
    height: 60px;
    width: 100%;
    position: fixed;
    left: 0;
    top: 0;
    z-index: 5;
    padding: 0 10px;
    display: flex;
    align-items: center;
    box-shadow: 0 2px 7px 2px rgb(51, 51, 51);
  }
  .nav-tabs{
    border: none;
  }
  .nav-tabs .nav-link.disabled{
    color: black;
  }
</style>

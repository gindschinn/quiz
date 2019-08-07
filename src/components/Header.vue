<template>
  <div class="header_container">
    <b-nav tabs>
      <b-nav-item disabled><b>Quiz App</b></b-nav-item>
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
    height: 60px;
    width: 100%;
    position: fixed;
    left: 0;
    top: 0;
    z-index: 5;
  }
</style>

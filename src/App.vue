<template>
  <div id="container">
    <h1>Calculadora</h1>
    <Calculator/>
  </div>
</template>

<script>
  import Calculator from './main/Calculator'

  export default {
    components: { Calculator }
  }
</script>

<style>
  * { 
    font-family: 'Roboto', sans-serif;
  }

  body {
    margin: 0;
    padding: 0;
  }

  #container {
    display: flex;
    flex-direction: column;
    height: 100vh;
    justify-content: center;
    align-items: center;
    text-align: center;

    color: #fff;
    background: linear-gradient(to right, rgb(83,105,110), rgb(41,46,73))
  }
</style>

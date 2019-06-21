<template>
  <div id="app">
    <div class="title-border">
      <div class="title">
        <h1>敲七训练器</h1>
      </div>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top:30px;
  }
  .title-border{
    width: 450px;
    margin: 0 auto;
    height: 100px;
    background: #FFB90F;
    display: flex;
    justify-content: center;
    align-items: center;
    border: 1px solid #000;
  }
  .title{
    background: #ff0000;
    height: 80px;
    display: flex;
    width: 95%;
    border: 1px solid #000;
    justify-content: center;
    align-items: center;
  }
  .title h1 {
    color: #fff;
    height: 80px;
    line-height: 80px;
  }
</style>

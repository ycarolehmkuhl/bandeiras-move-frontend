<template>
  <v-main>
    <div class="nav-bar">
      <a href="/">
        <h2 class="bar-title"></h2>
      </a>
      
    
    </div>
    <div class="container">
      <router-view></router-view>
    </div>
  </v-main>
</template>

<script>
export default {
  name: "App",
};
</script>

<style scoped>

*{
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

.container {
  padding-left: 10%;
  padding-right: 10%;
  background-color: #ffffff;
}

.nav-bar {
  padding-left: 10%;
  padding-right: 10%;
  height: 50px;
  width: 100%;
  display: grid;
  align-items: center;
  grid-template-columns: 2fr 1fr;
  background-color: rgb(255, 255, 255);
  color: #ccc;
}

.options ul{
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  font-size: 15px;
  list-style: none;
}

.options ul li{
  transition: .2s;
}

.options ul li:hover{
    cursor: pointer;
    color: #fff;
}

.container{
  padding-top: 20px;
}

.nav-bar a{
  text-decoration: none;
  color: #ccc;
}
</style>

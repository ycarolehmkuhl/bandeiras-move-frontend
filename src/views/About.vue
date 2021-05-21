<template>
  <div class="about">
    <a href="/">
      <Button txt="Voltar" />
    </a>
    <div class="country-container">
      <div class="img-container">
        <img class="country-img" :src="pais.flag" alt="" />
      </div>
      <div class="country-info">
        <div class="info-line name">
          <div class="info-title">Nome:</div>
          {{ pais.name }}
        </div>
        <div class="info-line capital">
          <div class="info-title">Capital:</div>
          {{ pais.capital }}
        </div>
        <a :href="'/?' + pais.region"><div class="info-line region">
          <div class="info-title">Região:</div>
          {{ pais.region }}
        </div></a>
        
        <div class="info-line subregion">
          <div class="info-title">Subregião:</div>
          {{ pais.subregion }}
        </div>
        <div class="info-line population">
          <div class="info-title">População:</div>
          {{ pais.population.toLocaleString("pt-BR") }} habitantes
        </div>
      </div>
    </div>
    <div class="other-info">
      <div class="info-line langs">
        <div class="info-title">Línguas: </div>{{langs}}
      </div>
    </div>
    <h3 class="list-title">Paises Vizinhos</h3>
    <CountryList page="About" ref="setUrl"/>
  </div>
</template>

<script>
// @ is an alias to /src
import Button from "../components/Button";
import CountryList from "../components/CountryList";


export default {
  name: "About",
  data: function () {
    return {
      pais: {
        population: ""
      },
      langs: "",
    };
  },
  components: {
    Button,
    CountryList
  },
  mounted: function () {
    // Select dos países da API, e atribuição dos mesmos a variável "paises"
    const urlAtual = window.location.href;
    let param = urlAtual.split("?");
    param = param[param.length - 1];

    const url = "https://restcountries.eu/rest/v2/alpha/" + param;

    fetch(url)
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        let langs = ""
        this.pais = data;
        let i = 0;
        this.pais.languages.map(function (lang) {
          console.log(lang.name)
          if(i != 0){
            langs = langs + ', ' 
          }
          langs = langs + lang.name;
          i++;
        });
        this.langs = langs
         this.$refs.setUrl.bordersSearch(this.pais.borders);
       
      });
     
  },
};
</script>

<style scoped>
.country-container {
  margin-top: 20px;
  display: grid;
  grid-template-columns: 2fr 3fr;
  margin-bottom: 10px;
}

.country-img {
  width: 100%;
}

.info-line {
  display: flex;
  padding: 10px;
  border: 1px solid #1f1f1f;
  border-top: 0;
}

.info-line:first-child {
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
  border-top: 1px solid #1f1f1f;
}
.info-line:last-child {
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
}

.info-title {
  font-weight: 600;
  margin-right: 10px;
}

.country-info {
  margin-left: 10px;
}

.region{
  transition: .2s;
  cursor: pointer;
  border-top: 0 !important;
  border-radius: 0 !important;
}

.region:hover{
  background-color: #ddd;
}

.country-info a{
  text-decoration: none;
  color: #000;
}

.other-info{
  margin-bottom: 20px;
}

.list-title{
  margin-bottom: 20px;
}
</style>


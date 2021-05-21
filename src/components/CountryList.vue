<template>
  <div
    v-bind:class="{
      multipleCountry: quantidadePaises > 1,
      fullScreenHome: quantidadePaises == 0 && page === 'Home',
    }"
    class="country-list"
  >
    <div v-if="quantidadePaises == 1">
      <a :href="'about?' + searchPaises[0].alpha2Code">
        <img class="country-flag" :src="searchPaises[0].flag" alt="" />
      </a>
    </div>
    <div v-if="quantidadePaises > 1">
      <div class="pagination">
        <div v-on:click="pagChange('first')">«</div>
        <div
          v-bind:class="{ activePag: pag === pagination }"
          v-on:click="pagChange(pag)"
          v-for="pag in this.pagArray"
          :key="pag"
        >
          {{ pag }}
        </div>
        <div v-on:click="pagChange('last')">»</div>
      </div>
      <div v-bind:class="{listContainerAbout: page == 'About'}" class="list-container">
        <div
        v-bind:class="{flagsContainerAbout: page == 'About'}"
          class="flags-container"
          v-for="item in searchPaises"
          :key="item.alpha2Code"
        >
          <a :href="'about?' + item.alpha2Code">
            <img
            v-bind:class="{countryFlagsAbout: page == 'About'}"
              class="country-flags"
              v-if="
                item.id <= pagination * 10 && item.id > pagination * 10 - 10
              "
              :src="item.flag"
              alt=""
            />
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CountryList",
  data: function () {
    return {
      searchPaises: {},
      quantidadePaises: 0,
      pagination: 1,
      pagArray: [],
    };
  },
  props: ["reqUrl", "page", "borders"],
  methods: {
    //Faz a busca na API pelas definições selecionadas
    searchCountry(value) {
      this.$emit("changed", true);
      fetch(value)
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          if (data.length > 0) {
            this.searchPaises = data;
          } else {
            this.searchPaises[0] = data;
          }
          this.quantidadePaises = 0;
          if (data.length > 1) {
            this.quantidadePaises = 2;
          } else {
            this.quantidadePaises = 1;
          }
          this.$emit("changed", false);
          let i = 1;
          this.searchPaises.map(function (pais) {
            pais.id = i;
            i++;
          });

          //Cria o array de paginação
          let array = [];
          for (i = 0; i < this.searchPaises.length; i++) {
            if (i % 10 == 0) {
              array.push(i / 10 + 1);
            }
          }
          this.pagination = 1;
          this.pagArray = array;
        });
    },

    pagChange(value) {
      if (value == "first") {
        this.pagination = 1;
      } else if (value == "last") {
        this.pagination = this.pagArray.length;
      } else {
        this.pagination = value;
      }
    },

    bordersSearch(value) {
      fetch('https://restcountries.eu/rest/v2/all')
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          let paises = []
          data.map(function(pais){
            value.map(function(region){
              if(pais.alpha3Code == region){
                paises.push(pais)
              }
            })
          })

          this.searchPaises = paises
        
          this.quantidadePaises = 2;
        
          this.$emit("changed", false);

          let i = 1;
          this.searchPaises.map(function (pais) {
            pais.id = i;
            i++;
          });

          //Cria o array de paginação
          let array = [];
          for (i = 0; i < this.searchPaises.length; i++) {
            if (i % 12 == 0) {
              array.push(i / 12 + 1);
            }
          }
          this.pagination = 1;
          this.pagArray = array;
        });
    },
  },
};
</script>

<style scoped>
.fullScreenHome {
  height: calc(100vh - 281px);
}

.country-flag {
  width: 100%;
  transition: 0.2s;
}

.list-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.listContainerAbout{
  grid-template-columns: 1fr 1fr 1fr 1fr ;
}

.flags-container {
  width: 98%;
}

.flags-container:nth-child(odd) {
  margin-right: 1%;
}
.flags-container:nth-child(even) {
  margin-left: 1%;
}

.flagsContainerAbout:nth-child(n){
  margin-right: 1%;
  margin-left: 1%;
}


.country-flags {
  transition: 0.2s;
  width: 100%;
  height: 25vw;
  border: 1px solid #1f1f1f;
  margin-bottom: 1vw;
}

.countryFlagsAbout{
  height: 12vw;
}

.country-flags:hover,
.country-flag:hover {
  filter: brightness(80%);
  cursor: pointer;
}

.country-list {
  padding-bottom: 50px;
}

.multiple-country {
  display: flex;
}

.pagination {
  display: flex;
  margin-bottom: 10px;
  
}

.pagination div {
  transition: 0.2s;
  padding: 10px 15px;
 
}

.pagination div:hover {
  background-color: #ffffff;
  border-style:solid;
  border-color: #1f1f1f;
  color: rgb(0, 0, 0);
  border-width:2px;
  cursor: pointer;
}

.activePag:hover {
  background-color: #6d2080b6 !important;
  
}

.activePag {
  background-color: #6D2080;
  color: #fff;
}
</style>

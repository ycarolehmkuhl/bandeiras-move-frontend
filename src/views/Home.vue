<template>
  <div class="home">
    <ul v-if="false" class="list">
      <li v-for="item in paises" :key="item.id">
        {{ item.name }}
      </li>
    </ul>
    
    <h2 class="search-title">Selecione os filtros para a pesquisa:</h2>
    <div class="filter-container">
      <Select
        @selected="filterSelected"
        id="filter-select"
        name="filter-list"
        :options="listOptions"
      />
      <div>
        <Select
          @selected="countrySelected"
          name="country-select"
          id="final-select"
          :options="list2Options"
        />
      </div>
    </div>
    <div class="button-container">
      <div v-if="spinner" class="spinner"></div>
      <Button @clicked="searchCountries" txt="Pesquisar" />
    </div>
    <CountryList page="Home" @changed="toggleSpinner" ref="setUrl" />
  </div>
</template>

<script>
import Select from "../components/Select";
import Button from "../components/Button";
import CountryList from "../components/CountryList";

export default {
  name: "Home",
  data: function () {
    return {
      paises: {},
      //Inicialização das opções para a primeira lista de filtro
      listOptions: [
        { value: "", option: "Selecione o tipo de filtro desejado" },
        { value: "region", option: "Região" },
        { value: "capital", option: "Capital" },
        { value: "languages", option: "Língua" },
        { value: "name", option: "País" },
        { value: "callingCodes", option: "Código de Ligação" },
      ],
      //Inicialização da lista vazia do 2º Select
      list2Options: [{ value: "", option: "Selecione na primeira lista" }],
      urlParam: "",
      realUrl: "",
      spinner: false,
    };
  },
  components: {
    Select,
    Button,
    CountryList,
  },
  methods: {
    filterSelected(value) {
      //Criação da lista do 2º select a partir da seleção do primeiro
      let options = [];
      options.push({ value: "", option: "Selecione o desejado" });

      let items = [];
      // Criação de lista de linguas
      if (value == "languages") {
        this.paises.map(function (pais) {
          if (pais[value]) {
            pais[value].map(function (val) {
              items.push(val.name);
            });
          }
        });

        //Criação da lista de Códigos de ligação
      } else if (value == "callingCodes") {
        this.paises.map(function (pais) {
          if (pais[value].length > 0) {
            pais[value].map(function (val) {
              items.push(parseInt(val));
            });
          }
        });

        // Criação das outras listas
      } else {
        this.paises.map(function (pais) {
          if (pais[value]) {
            items.push(pais[value]);
          }
        });
      }

      // Definição do primeiro valor pro parêmetro do Link de pesquisa
      this.urlParam = value;

      // Retirada de items repetidos

      items = items.filter((item, i) => items.indexOf(item) === i);

      //Ordenação da lista
      if (value == "callingCodes") {
        items.sort(function (a, b) {
          return a - b;
        });
      } else {
        items.sort();
      }

      items.map(function (item) {
        options.push({ value: item, option: item });
      });

      this.list2Options = options;
    },

    countrySelected(value) {
      // Definição do URL para cada parâmetro, e requisição nos mesmos
      if (this.urlParam == "region") {
        this.realUrl = "https://restcountries.eu/rest/v2/region/" + value;
      } else if (this.urlParam == "capital") {
        this.realUrl = "https://restcountries.eu/rest/v2/capital/" + value;
      } else if (this.urlParam == "languages") {
        let param = "";
        this.paises.map(function (pais) {
          pais.languages.map(function (lang) {
            if (lang.name == value) {
              param = "https://restcountries.eu/rest/v2/lang/" + lang.iso639_1;
            }
          });
        });
        this.realUrl = param;
      } else if (this.urlParam == "name") {
        let param = "";
        this.paises.map(function (pais) {
          if (pais.name == value) {
            param = "https://restcountries.eu/rest/v2/alpha/" + pais.alpha2Code;
          }
        });
        this.realUrl = param;
      } else if (this.urlParam == "callingCodes") {
        let param = "";
        this.paises.map(function (pais) {
          pais.callingCodes.map(function (code) {
            if (code == value) {
              param = "https://restcountries.eu/rest/v2/region/" + pais.region;
            }
          });
        });
        this.realUrl = param;
      }

      //Parada aqui, fazendo a pesqusa pelo nome (Currency)
    },
    searchCountries(url) {
      if (url) {
        this.$refs.setUrl.searchCountry(url);
      } else {
        this.$refs.setUrl.searchCountry(this.realUrl);
      }
    },
    toggleSpinner(value) {
      console.log(value);
      this.spinner = value;
    },
  },
  mounted: function () {
    // Select dos países da API, e atribuição dos mesmos a variável "paises"

    const actualUrl = window.location.href;
    const url = `https://restcountries.eu/rest/v2/all`;

    fetch(url)
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.paises = data;
        let i = 0;
        this.paises.map(function (pais, i) {
          pais.id = i;
        });
        let region = actualUrl.split("?")[1];

        if (region) {
          region = region.split("#")[0];
          let searchUrl = "https://restcountries.eu/rest/v2/region/" + region;
          this.searchCountries(searchUrl);
        }
      });
  },
};
</script>

<style scoped>
.search-title {
  padding: 20px 0;
}

.filter-container {
  width: 100%;
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.button-container {
  display: flex;
  justify-content: flex-end;
  padding: 30px 0;
}

.spinner {
  margin-right: 20px;
  border: 7px solid rgba(0, 0, 0, 0.1);
  border-left-color: #1f1f1f;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
</style>

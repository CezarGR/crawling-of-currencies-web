<template>
  <NavbarComponent/>
  <div class="home">
    <div class="container-search-currey">
      <h4>Pesquise moedas pelo <span class="color-orange">código</span> ou <span class="color-orange">número</span></h4>
      <input type="text" v-model="values">
      <div>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="inlineRadioOptions" id="inlineRadio1" value="code" v-model="searchBy"/>
          <label class="form-check-label" for="inlineRadio1">código</label>
        </div>
        <div class="form-check form-check-inline">
          <input class="form-check-input" type="radio" name="inlineRadioOptions" id="inlineRadio2" value="number" v-model="searchBy">
          <label class="form-check-label" for="inlineRadio2">número</label>
        </div>
        <div class="d-grid gap-2 col-6 mx-auto">
          <button @click="searchCurrency()" class="search-button btn btn-primary" type="button">Buscar</button>
        </div>
      </div>
    </div>
    <div class="container-currencies">
      <div v-for="currency in currencies">
        <CurrencyComponent
          :numero= currency.number
          :currencyName= currency.name
          :code= currency.code 
          :symbol= currency.symbol 
          :iconUrl=currency.locations
        />
      </div>
    </div>
  </div>
</template>

<script>
import CurrencyComponent from '@/components/CurrencyComponent.vue';
import NavbarComponent from '@/components/NavbarComponent.vue';

export default {
  name: 'HomeView',
  data() {
    return {
      searchBy: "code",
      values: '',
      currencies: []
    }
  },
  components: {
    NavbarComponent,
    CurrencyComponent
  },
  methods: {
    setItems(request) {
      request = request.currentTarget
      if (request.status == 200) {
          let response = JSON.parse(request.responseText);
          let currencies = Object.values(response.data)
          currencies.forEach(item => {
            this.currencies.push(item);
          });
      } else {
          alert('ERROR');
      }
    },
    searchCurrency() {
      let request = new XMLHttpRequest();
      let valuesArray = this.values.toUpperCase().replace(' ', '').split(',');
      let body = this.searchBy == 'code' ? {"codes": valuesArray} : {"numbers": valuesArray}
      request.open("POST", "http://localhost:8083/api/v2/currencies/search");
      request.setRequestHeader('Content-Type', 'application/json');
      request.addEventListener("load", this.setItems);
      request.send(JSON.stringify(body));
    }
  }
}
</script>

<style>
  h4 {
    margin: 30px;
    font-weight: 400;
  }

  input {
    border: none;
    border-bottom: 2px solid;
    text-align: center;
    text-transform: uppercase;
  }

  input:focus {
    box-shadow: 0 0 0 0;
    outline: 0;
  }

  .container-search-currey {
    display: flex;
    flex-direction: column;
    flex-wrap: nowrap;
    align-content: center;
    justify-content: center;
    align-items: center;
  }

  .form-check-input:checked {
    background-color: #FF8400;
    border-color: #FF8400;
  }

  .container-currencies {
    margin-top: 50px;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-content: center;
    justify-content: center;
    align-items: center;
  }

  .search-button {
    margin: 20px;
    background-color: #FF8400;
    border: none;
  }

  .search-button:hover {
    background-color: #a75703;
  }
</style>

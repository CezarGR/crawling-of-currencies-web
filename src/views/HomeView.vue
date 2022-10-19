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
          :number= currency.number
          :currencyName= currency.name
          :code= currency.code 
          :symbol= currency.symbol 
          :iconUrl=currency.locations
        />
      </div>
    </div>
    <div id="alert-message-container"></div>
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
    addCurrencies(currencies) {
      currencies.forEach(currency => {
        this.currencies.push(currency);
      });
    },
    requestSearchCurrency(values) {
        const request = new XMLHttpRequest();
        let body = this.searchBy == 'code' ? {"codes": values} : {"numbers": values}
        request.open("POST", "http://localhost:8083/api/v2/currencies/search");
        request.setRequestHeader('Content-Type', 'application/json');
        request.addEventListener("load", this.responseSearchCurrency);
        request.send(JSON.stringify(body));
    },
    responseSearchCurrency(request) {
      request = request.currentTarget
      if (request.status == 200) {
          this.addCurrencies(
            Object.values(JSON.parse(request.responseText).data)
          );
          this.alertSuccess(`Moedas adicionadas com sucesso`);
      } else {
          let message = JSON.parse(request.responseText).message
          this.alertErro(message ?? 'Erro ao utilizar serviço de rastreamento')
      }
    },
    searchCurrency() {
      let valuesArray = this.values.toUpperCase().replace(' ', '').split(',');
      let searchBy = this.searchBy;

      let duplicateCurrencies = this.currencies.filter(function (item) {
        return valuesArray.includes(item.code) || valuesArray.includes(item.number);
      })

      duplicateCurrencies.forEach(function (item) {
        let cardCurrency = document.getElementById(`card-currency-${item.number}`);
        setTimeout(() => cardCurrency.classList.add('card-currey-selected'), 100)
        cardCurrency.classList.remove('card-currey-selected');

        valuesArray = valuesArray.filter(function (value) {
          return (searchBy === 'number' && value != item.number) || (searchBy === 'code' && value != item.code);
        });
      });

      if (valuesArray.length > 0) {
        this.requestSearchCurrency(valuesArray);
      }
    },
    alertSuccess(message) {
      const alertPlaceholder = document.getElementById('alert-message-container')
      const wrapper = document.createElement('div')
      wrapper.innerHTML = [
        `<div class="alert alert-success alert-dismissible" role="alert" style="background-color: #1987545c;">`,
        `   <div>${message}</div>`,
        '   <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>',
        '</div>'
      ].join('')

      alertPlaceholder.append(wrapper)
    },
    alertErro(message) {
      const alertPlaceholder = document.getElementById('alert-message-container')
      const wrapper = document.createElement('div')
      wrapper.innerHTML = [
        `<div class="alert alert-error alert-dismissible" role="alert" style="background-color: #ff000070;">`,
        `   <div>${message}</div>`,
        '   <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>',
        '</div>'
      ].join('')

      alertPlaceholder.append(wrapper)
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

  .card-currey-selected {
    background-color: transparent;
    animation: card-currey-selected-animation 1s alternate;
  }

  @keyframes card-currey-selected-animation {
    0% {
      background-color: #ff84000e;
    }
    25% {
      background-color: #ff84001f
    }
    50% {
      background-color: #ff840041;
    }
    75% {
      background-color: #ff84001f;
    }
    100%{
      background-color: #ff84000e;
    }
  }

  #alert-message-container {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    flex-wrap: nowrap;
    justify-content: center;
  }

  #alert-message-container > div {
    width: 50%;
  }
</style>

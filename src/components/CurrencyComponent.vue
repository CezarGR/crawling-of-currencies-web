<template>
  <div :id="setId()" class="card" style="width: 18rem;">
    <div class="card-body">
      <h5 class="card-title">{{currencyName}}</h5>
      <p class="card-text"><span class="color-orange">Código:</span> {{code}}</p>
      <p class="card-text"><span class="color-orange">Número:</span> {{number}}</p>
      <p class="card-text"><span class="color-orange">Simbolo:</span> {{symbol}}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CurrencyComponent',
  data() {
    return {
      item: this.number
    }
  },
  props: [
    'number', 'currencyName', 'code', 'symbol', 'iconUrl' 
  ],
  methods: {
    setId() {
      return `card-currency-${this.number}`;
    }
  }
}
</script>

<style>
  .card {
    margin: 10px;
  }

  .card > .card-body > p {
    text-align-last: left;
    margin: 0px;
  }
</style>
<template>
  <div class="coin">
      <h1>Currency Values</h1>
      <table>
          <thead>
              <tr>
                  <th>Type</th>
                  <th>New Value</th>
                  <th>Last Value</th>
                  <th>Difference</th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="(currency, index) in currencies" :key="index">
                  <td>{{currency.type}}</td>
                  <td>{{currency.value}}</td>
                  <td>{{currency.oldValue}}</td>
                  <td>{{currency.diff}}</td>
              </tr>
          </tbody>
      </table>
  </div>
</template>

<script>
const remote = window.require("electron").remote;
const dialog = remote.dialog;

export default {
  data() {
    return {
      oldCurrencies:
        typeof Storage != undefined && localStorage.currencies
          ? JSON.parse(localStorage.getItem("currencies"))
          : [],
      currencies: []
    };
  },
  mounted() {
    this.getCurrencyValues();
  },
  methods: {
    getCurrencyValues() {
      fetch(
        "http://cors-anywhere.herokuapp.com/www.ecb.europa.eu/stats/eurofxref/eurofxref-daily.xml"
      )
        .then(res => res.text())
        .then(str => new window.DOMParser().parseFromString(str, "text/xml"))
        .then(data => {
          this.currencies = [
            ...data.getElementsByTagName("Cube")[0].getElementsByTagName("Cube")
          ].map(
            node =>
              new Object({
                type: node.getAttribute("currency"),
                value: node.getAttribute("rate")
              })
          );

          this.currencies.shift();

          // update locale storage
          if (Storage != undefined) {
            localStorage.currencies = JSON.stringify(this.currencies);
          }

          this.currencies.map((currency, index) => {
            currency.oldValue = this.oldCurrencies[index].value;
            currency.diff = currency.oldValue - currency.value;

            return currency;
          });
        });
    }
  }
};
</script>

<style scoped>
.coin {
  text-align: center;
  margin: 0 auto;
}
</style>

<template>
  <div>
    <h2 class="header">Currency Converter</h2>
    <div>
      <label for="amount">Amount:</label>
      <input type="number" v-model="amount" id="amount" class="input-field" />
    </div>
    <div>
      <label for="from">From:</label>
      <select v-model="from" id="from" class="select-field">
        <option v-for="currency in currencies" :key="currency" :value="currency">
          {{ currency }}
        </option>
      </select>
    </div>
    <div>
      <label for="to">To:</label>
      <select v-model="to" id="to" class="select-field">
        <option v-for="currency in currencies" :key="currency" :value="currency">
          {{ currency }}
        </option>
      </select>
    </div>
    <div>
      <button @click="convert" :class="{ 'loading': isLoading }">Convert</button>
    </div>
    <div v-if="result">
      <p> {{ result }}</p>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';


export default {
  name: 'CurrencyConverter',
  setup() {
    const amount = ref(0);
    const from = ref('USD');
    const to = ref('EUR');
    const result = ref('');
    const isLoading = ref(false);

    const currencies = ['USD', 'EUR', 'GBP', 'NGN', 'ZAR', 'GHS', 'GBP', 'CAD', 'JPY']; // Add more currencies as needed

    const convert = async () => {
      try {
        isLoading.value = true;

        const response = await fetch(
          `https://api.exchangerate-api.com/v4/latest/${from.value}`
        );
        const data = await response.json();
        const rate = data.rates[to.value];
        const convertedAmount = (amount.value * rate).toFixed(2);
        result.value = `${amount.value} ${from.value} = ${convertedAmount} ${to.value}`;
      } catch (error) {
        console.error(error);
      } finally {
        isLoading.value = false;
      }
    };

    return {
      amount,
      from,
      to,
      result,
      currencies,
      convert,
      isLoading,
    };
  },
};
</script>

<style>

* {
font-family: Arial, Helvetica, sans-serif;
}
.header {
  color: white;
  padding: 10px;
  border-radius: 5px;
  margin: 6px -14px 21px -14px;
  font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  font-weight: 900;
  font-size:x-large;

}

.input-field {
  border-radius: 7px;
  width: 65%;
}

.select-field {
  border-radius: 5px;
}

button {
  border-radius: 5px;
  background-color:rgb(27, 173, 184);
  color: white;
  padding: 5px 10px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: rgb(255, 179, 0);
}

button.loading {
  position: relative;
}

button.loading::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border: 2px solid white;
  border-top-color: transparent;
  animation: spin 0.4s infinite linear;
}

@keyframes spin {
  0% {
    transform: translate(-50%, -50%) rotate(0deg);
  }
  100% {
    transform: translate(-50%, -50%) rotate(360deg);
  }
}
</style>
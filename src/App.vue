<template>
  <div id="app">
    <CurrencyConverter />
  </div>
</template>

<script>
import CurrencyConverter from './components/CurrencyConverter.vue';

export default {
  name: 'App',
  components: {
    CurrencyConverter,
  },
};
</script>

<style>
#app {
  text-align: center;
  padding: 20px;
}
</style>
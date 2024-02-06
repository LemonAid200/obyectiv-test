<template>
  <CustomSelect :optionsList="citiesList"></CustomSelect>
</template>

<script>

import CustomSelect from './components/UI/customSelect.vue';

export default {
  name: 'App',
  components: {
    CustomSelect
  },
  data: () => {
    return {
      citiesList: ['Краснодарский край', 'Красноярский край', 'Пермский край']
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

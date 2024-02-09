<template>
  <CustomTable> </CustomTable>
  <br>
  <CustomSelect v-on:updateOption="(option) => chosenCity = option" :optionsList="citiesList" :placeholder="placeholder" :selectedOption="chosenCity"></CustomSelect>
</template>

<script>

import CustomSelect from './components/UI/customSelect.vue';
import CustomTable from './components/UI/customTable.vue';

export default {
  name: 'App',
  components: {
    CustomSelect,
    CustomTable
  },
  data: () => {
    return {
      citiesList: [
        {id: 1, title: 'Краснодарский край'},
        {id: 2, title: 'Красноярский край'},
        {id: 3, title: 'Пермский край'}
      ],
      placeholder: 'Выберите название региона',
      chosenCity: {}
    }
  },

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

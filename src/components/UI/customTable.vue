
<script setup>
import { ref } from "@vue/reactivity";
import {  computed } from 'vue'

const data = [
  {
    id: 1,
    number: 421,
    developer: 'PRINZIP',
    deadline: '2005-08-09T18:31:42',
    type: 'Студия',
    floor: 2,
    square: 100.3
  },
  {
    id: 2,
    number: 73,
    developer: 'Брусника',
    deadline: '2005-08-09T18:31:42',
    type: '2-к',
    floor: 2,
    square: 10.3
  },
  {
    id: 3,
    number: 122,
    developer: 'TEN',
    deadline: '2005-08-09T18:31:42',
    type: '3-к',
    floor: 2,
    square: 103
  },
  {
    id: 4,
    number: 1,
    developer: 'PRINZIP',
    deadline: '2005-08-09T18:31:42',
    type: 'Студия',
    floor: 2,
    square: 100.3
  },
  {
    id: 5,
    number: 72,
    developer: 'Брусника',
    deadline: '2005-08-09T18:31:42',
    type: '2-к',
    floor: 2,
    square: 10.3
  },
  {
    id: 6,
    number: 23,
    developer: 'TEN',
    deadline: '2005-08-09T18:31:42',
    type: '3-к',
    floor: 2,
    square: 103
  },
  {
    id: 7,
    number: 5,
    developer: 'PRINZIP',
    deadline: '2005-08-09T18:31:42',
    type: 'Студия',
    floor: 2,
    square: 100.3
  },
  {
    id: 8,
    number: 2,
    developer: 'Брусника',
    deadline: '2005-08-09T18:31:42',
    type: '2-к',
    floor: 2,
    square: 10.3
  },
  {
    id: 9,
    number: 97,
    developer: 'TEN',
    deadline: '2005-08-09T18:31:42',
    type: '3-к',
    floor: 2,
    square: 103
  },
  {
    id: 10,
    number: 34,
    developer: 'PRINZIP',
    deadline: '2005-08-09T18:31:42',
    type: 'Студия',
    floor: 2,
    square: 100.3
  },
  {
    id: 11,
    number: 1,
    developer: 'Брусника',
    deadline: '2005-08-09T18:31:42',
    type: '2-к',
    floor: 2,
    square: 10.3
  },
  {
    id: 12,
    number: 88,
    developer: 'TEN',
    deadline: '2005-08-09T18:31:42',
    type: '3-к',
    floor: 2,
    square: 103
  }
]
const titlesKeys = {
  ['номер']: 'number',
  ['девелопер']: 'developer',
  ['срок']: 'deadline',
  ['тип']: 'type',
  ['этаж']: 'floor',
  ['площадь']: 'square'
}
const titles = ['номер','девелопер','срок','тип','этаж','площадь']

let sortKey = ref('')
let searchParam = ref('')
let isSortReverse = ref(1)
let page = ref(1)
let amountOfEntriesToShow = ref(10)

function setSortKey(columnName){
  if (sortKey.value === titlesKeys[columnName]){
    isSortReverse.value = -1 * isSortReverse.value
    return
  }
  isSortReverse.value = 1
  sortKey.value = titlesKeys[columnName]
  page.value = 1
}

function flipPage(newPageNum){
  if (newPageNum < 1 || newPageNum - 1 >= amountOfEntries.value / amountOfEntriesToShow.value) return
  page.value = newPageNum
}

function updatePagesOnAmountChange(){
  if (entriesToShow.value.length !== 0 && data.length === 0) return
  page.value = 1
}

function compare( a, b ) {
  if ( a[sortKey.value] < b[sortKey.value] ){
    return -1 * isSortReverse.value;
  }
  if ( a[sortKey.value] > b[sortKey.value] ){
    return 1 * isSortReverse.value;
  }
  return 0;
}
function isISO8601Date(dateString) {
  const iso8601Pattern = /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}$/;
  return iso8601Pattern.test(dateString);
}


const amountOfEntries = computed(() => {
  return filteredSortedEntries.value.length
})
let start = computed(() => {
  return  1 + amountOfEntriesToShow.value * (page.value - 1)
})
let end = computed(() => {
  return start.value + amountOfEntriesToShow.value - 1 < amountOfEntries.value ? start.value + amountOfEntriesToShow.value - 1 : amountOfEntries.value
})

let sortedEntries = computed(() => {
  if (!sortKey.value)	return data
  let sortedData = Array.from(data)
  if (isISO8601Date(data[0][sortKey.value])){
    const collator = new Intl.Collator(undefined, {numeric: true, sensitivity: 'base'});
    sortedData.sort((a, b) => collator.compare(a[sortKey.value], b[sortKey.value]));
    if (isSortReverse.value === -1){
      sortedData.reverse()
    }
  } else {
    sortedData.sort(compare)
  }
  return sortedData
})

let filteredSortedEntries = computed(() => {
  return sortedEntries.value.filter((entry) => {
    for (let key in entry){
      if (entry[key].toString().toUpperCase().includes(searchParam.value.toUpperCase())) return true
    }
    return false
  })
})

let entriesToShow = computed(() => {
  return filteredSortedEntries.value.slice(start.value - 1, end.value)
})

</script>

<template>
	<div class="custom_table">
		<div class="table_control">
		<div class="table_control__amount">
			Show 
			<select v-model="amountOfEntriesToShow" @change="updatePagesOnAmountChange">
				<option v-for="num in amountOfEntries" :key="num" :value="num" >{{ num }}</option>
			</select>
			entries
		</div>
		<div class="table_control__search">
			Search: <input type="text" v-model="searchParam">
		</div>
	</div>
  <div v-if="!entriesToShow.length" class="table-empty">No entries available</div>

	<table v-else>
		<tr class="table_titles">
			<th v-for="title in titles" :key="title" @click="setSortKey(title)" :class="{selected_column: sortKey === titlesKeys[title]}">
        <span class="column_sort_icon">
          <svg class="svg-icon" style="width: 10px; height: 10px; vertical-align: middle;fill: currentColor;overflow: hidden;" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"><path d="M512.03799 0 921.675981 411.648 102.4 411.648 512.03799 0ZM512.03799 995.328 102.4 583.68 921.675981 583.68 512.03799 995.328Z"  /></svg>
        </span>
        {{ title }}
      </th>
		</tr>

		<table class="table_content">
			<tr v-for="{ id,  number, developer, deadline, type, floor, square } in entriesToShow" :key="id">
				<td>{{ number }}</td>
				<td>{{ developer }}</td>
				<td>{{ deadline }}</td>
				<td>{{ type }}</td>
				<td>{{ floor }}</td>
				<td>{{ square }}</td>
			</tr>			
		</table>

		<tr class="table_titles">
			<th v-for="title in titles" :key="title" @click="setSortKey(title)" :class="{selected_column: sortKey === titlesKeys[title]}">{{ title }}</th>
		</tr>
  </table>

	<div class="table_pagination">
		<span>Showing {{ start }} to {{ end }} of {{ amountOfEntries }} entries</span>
		<div class="table_pagination__flip">
			<button class="table_pagination__flip__previous" @click="() => { flipPage(page - 1) }">
				<span class="arrow">
					<svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
						<path d="M3 6L8 11L13 6" stroke="#BBC3FF" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
					</svg>		
				</span>
				Previous
			</button>  
      <button class="table_pagination__flip__next" @click="() => { flipPage(page + 1) }">
				Next
				<span class="arrow">
					<svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
						<path d="M3 6L8 11L13 6" stroke="#BBC3FF" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
					</svg>		
				</span>				
			</button>
		</div>
	</div>
	</div>
</template>

<style scoped>
.custom_table{
	margin: auto;
	max-width: fit-content;
}
.table_control {
	display: flex;
	justify-content: space-between;
	gap: 100px;
  color: #b7afaf;
}
.table_pagination {
	display: flex;
	justify-content: space-between;
  color: #b7afaf;
}
.table_pagination__flip button {
	border: none;
	background: none;
  color: #b7afaf;
}
.table_pagination__flip .arrow {
	vertical-align: middle;
}
.table_pagination__flip__previous .arrow svg{
	transform: rotate(90deg);
}
.table_pagination__flip__next .arrow svg{
	transform: rotate(-90deg);
}

.table-empty{
  padding: 10px;
  background: #eb9797;
  margin: 10px;  
}

.table_titles {
	background-color: #cff2ff85;
}

tr{
  border-top: 1px solid rgba(128, 128, 128, 0.521);
  border-bottom: 1px solid rgba(128, 128, 128, 0.521);
}
th {
  cursor: pointer;
}
th, td {
	padding: 5px;
	text-align: left;
}

table {
	margin: 10px auto;
  border-collapse: collapse;  
}
table tr{
  display: grid;
  gap: 10px;
  grid-template-columns: repeat(6, 1fr);
}

.table_content{
  max-height: 544px;
  overflow-y: auto;
  margin: 0 auto;
  display: block;
  text-wrap: nowrap;
  color: #b7afaf;
}

.selected_column svg path{
  fill: #29277d;
}
</style>
<script>
import { ref } from "@vue/reactivity";
export default {
	props: {
		optionsList: {
			type: Array,
			default: () => [{id: 1,	title: 'Слон'} ,	{id: 2,	title: 'Бегемот'} ,	{id: 3,	title: 'Зебра'} ,	{id: 4,	title: 'Жираф'},	{id: 5,	title: 'Лев'}]
		},
		selectedOption: {
			type: Object
		},
		placeholder: {
			type: String,
			default: () => 'Выберите название животного'
		}
	},
	emits: ['updateOption'],
	setup(props, context) {
		let isActive = ref(true)
		function updateOption(option){
			context.emit('updateOption', option)
			toggleSelect()
		}
		function toggleSelect(){
			isActive.value = !isActive.value;
		}
		return {isActive, toggleSelect, updateOption}
  }
}

</script>

<template>
	<div class="select_box" :class="{ select_box__active: isActive, select_box__hasChosenOption: selectedOption.title }">
		<div class="select_box__placeholder_top">{{ placeholder }}</div>
		<div class="select_box__main">
			<div class="select_box__placeholder" >{{ selectedOption.title || placeholder }}</div>
		<div class="select_box__arrow" @click.stop="toggleSelect">
			<svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
				<path d="M3 6L8 11L13 6" stroke="#BBC3FF" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
			</svg>		
		</div>

		</div>

		<ul class="select_box__list" >
			<li class="select_box__list__option" v-for="{title, id} in optionsList" :value="title" :key="id" @click.stop="updateOption({title, id})">{{title}}</li>
		</ul>
	</div>
</template>

<style scoped>
.select_box {
	background: white;
	width: fit-content;
	border-radius: 10px;
	border: 2px solid #DADEFE;
	padding: 0;
	color: #29277d;
	position: relative;
	font-size: 12px;
	min-width: 300px;
}
.select_box__placeholder_top{
	display: none;
}
.select_box__hasChosenOption .select_box__placeholder_top{
	display: block;
	color: rgba(41, 39, 125, 0.40);
	position: absolute;
	left: 4px;
	top: -8px;
	font-size: 11px;
	background: white;
	padding: 2px;
}
.select_box__main{
	display: flex;
	border-bottom: 2px solid #dadefe00;
	justify-content: space-between;
	transition: 0.5s;
}
.select_box__active .select_box__main{
	border-bottom: 2px solid #DADEFE;
}
.select_box__arrow{
	margin-right: 5px;
	margin-left: 40px;
}

.select_box__arrow{
	align-self: center;
	cursor: pointer;
}
.select_box__arrow svg path{
	stroke: #5F88F4;
}
.select_box svg{
	transition: 0.5s;
}
.select_box__active svg{
	transform: rotate(180deg);
	transition: 0.5s;
}
.select_box__placeholder {
	color: rgba(41, 39, 125, 0.40);
	padding: 10px;
}
.select_box__hasChosenOption .select_box__placeholder{
	color: #29277d;
}
.select_box__active .select_box__list {

}
.select_box__list {
	list-style-type: none;
	padding-left: 0;
	text-align: left;
	padding: 0;
	margin: 0;
	transition: 0.5s;
	max-height: 0vh;
	overflow: hidden;
	transition: 0.5s;
}
.select_box__active .select_box__list{
	margin-top: 10px;
	transition: 0.5s;
	max-height: 100vh;
}

.select_box__list__option {
	padding: 5px;
	overflow: hidden;
	cursor: pointer;
}
.select_box__active .select_box__list__option{
	padding: 5px;
}

.select_box__list__option:hover {
	background: #DADEFE;
}

.select_box__active{
}

</style>
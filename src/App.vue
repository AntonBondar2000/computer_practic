<template>
	<div class="tabs">
		<div 
			class="tabs__item" 
			:class="{active: currentTab=='Credit'}"
			@click="currentTab = 'Credit'"
		>
			Кредит
		</div>
		<div 
			class="tabs__item" 
			:class="{active: currentTab=='Mortgage'}"
			@click="currentTab = 'Mortgage'"
		>
			Ипотека
		</div>
		<div 
			class="tabs__item" 
			:class="{active: currentTab=='Deposit'}"
			@click="currentTab = 'Deposit'"
		>
			Вклад
		</div>
	</div>
	<div class="main_form">
		<component :is="currentTab" class="tab"></component>
	</div>
</template>

<script>
import Credit from './components/Credit.vue'
import Mortgage from './components/Mortgage.vue'
import Deposit from './components/Deposit.vue'

export default {
	name: 'App',
	components: {
		Credit,
		Mortgage,
		Deposit
	},
	data(){
		return {
			currentTab: 'Credit',
		}
	}
}
</script>

<style lang="scss">
*{
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
.tabs{
	width: 80%;
	display: flex;
	&__item{
		background-color: rgba(128, 128, 128);
		padding: 32px 50px 32px 50px;
		border-radius: 15px 15px 0 0;
		color: black;
		transition-duration: .3s;
		cursor: pointer;
		&:hover{
			background-color: #778899;
			color: black;
		}
	}
	.active{
		background-color: #778899;
		color: black;
	}
}
#app {
	width: 100%;
	height: 100vh;
	display: flex;
	align-items: center;
	flex-direction: column;
	justify-content: center;
}
.main_form{
	width: 80%;
	height: 700px;
	background: #778899;
}
</style>

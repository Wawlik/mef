<template>
	<div class="hello">
		<h1>{{ msg }}</h1>
			<input-group 
				v-if="inputData"
				:data="inputData"
				@focus-gained="colored = ind"
				@focus-lost="colored = null"
				></input-group>
	</div>
</template>

<script>
import inputGroup from './inputGroup.vue';
import data from '../../public/data/persons.json';

export default {
	name: 'HelloWorld',
	components: {
		inputGroup
	},
	props: {
		msg: String
	},
	data(){
		return{
			inputData: null,
		}
	},
	mounted(){
		this.inputData = data;
	}
}
</script>

<style>
	.hello{
		margin: auto;
	}
</style>

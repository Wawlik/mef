<template>
	<div class="hello">
	<h1>{{ msg }}</h1>
	<div v-for="one in persons" :key="'person-' + one.id" class="person">
		<img src="@/assets/child.jpg" alt="" class="person-avatar">
		<div class="person-info">
			<span class="person-info-name">{{one.name.toUpperCase()}} IS</span>
			<div class="person-info-input-group">
				<input-group :data="one"></input-group>
				<span class="person-info-input-prefix">{{one.prefix}}</span>
			</div>
		</div>
	</div>
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
			persons: []
		}
	}, 
	mounted(){
		this.persons = data;
	}
}
</script>

<style>
	.person{
		display: flex;
		margin-bottom: 20px;
		justify-content: flex-start;
		align-items: center;
		color: #2c2c30;
	}
	.hello{
		margin: auto;
	}
	.person-avatar{
		border-radius: 50%;
		width: 100px;
		height: 100px;
	}
	.person-info{
		margin-left: 20px;
		width: 100%;
		text-align: left;
	}
	.person-info-input-group{
		display: flex;
		margin-top:20px;
	}
	.person-info-input-prefix{
		margin-left: 10px;
	}
	.person-info-name{
        font-size: 13px;
		font-weight: bold;
	}
</style>

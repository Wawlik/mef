<template>
	<div class="hello">
		<h1>{{ msg }}</h1>
		<div v-for="(one, ind) in persons" :key="'person-' + one.id" :class="'person ' + (colored === ind ? 'person--colored':'')">
			<div class="person-avatar--wrapper">
				<img src="@/assets/hugh.png" alt="" class="person-avatar">
			</div>
			<div class="person-info">
				<span class="person-info-name">{{one.name.toUpperCase()}} IS</span>
				<div class="person-info-input-group">
					<input-group 
						:data="one"
						@focus-gained="colored = ind"
						@focus-lost="colored = null"
						></input-group>
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
			persons: [],
			colored: null
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
		font-family: 'Roboto', sans-serif;
	}
	.hello{
		margin: auto;
	}
	.person-avatar, .person-avatar--wrapper{
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
		transition: all 0.25s;
	}
	.person--colored > .person-info-name--colored{
		color: #2540ff;
	}
	.person--colored > .person-avatar--wrapper{
		border: 2px solid #2540ff;
		margin: -2px;
	}
	.person--colored > .person-info > .person-info-name{
		color: #2540ff;
	}
</style>

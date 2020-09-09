<template>
	<div class="inputGroup">
		<input type="text" :id="'input-' + data.id"
			v-model="count"
			@click="$emit('focus-gained')"
			@blur="$emit('focus-lost')"
			style="width:50px;min-width:50px!important"
			@input="checkText($event.target.value)">
	</div>
</template>

<script>

export default {
	name: 'inputGroup',
	props: {
		data: Object
	},
	data(){
		return{
			count: this.$props.data.count || 0
		}
	},
	computed:{
		isFocus(){
			const a = document.getElementById(this.$props.data.count);
			if(a){
				return 1;
			}
			return 2;
		},
	},
	methods: {
		checkText(val = this.$props.data.count) {
			// first enter
            if(typeof val !== 'number') {
				// replace all spaces
                val = val.replace(/\s/g, '');
			}
			console.log({val});
			let v = null;
			if(Number.isNaN(+val)) {
				// replace all letters
				v = +val.replace(/[^\d]/g,'');
			}
			v = v || val;
			// if(!Number.isNaN(+v)) {
			this.count = (+v).toLocaleString();
			this.autoSizeInput(this.count);
			// }
        },
        autoSizeInput(count) {
            const input = document.getElementById('input-'+this.$props.data.id);
            if(input) {
                input.style.width = ((count.length + 1) * 6.7) + 'px';
            }
        }
    },
    mounted(){
        this.checkText();
    }
}
</script>

<style scoped>
/* scoped to disable outer interactions */
	.inputGroup > input{
		border: none;
		border-bottom: 1px solid #c9c9cf;
		width: auto;
        resize: both;
        transition: all 0.25s;
        font-size: 14px;
        font-weight: bold;
        line-height: 1.21;
        color: #2c2c30;
	}
	.inputGroup > input:focus{
		border: none;
		outline: none;
		border-bottom: 1px solid #2540ff;
		caret-color: #2540ff;
	}
</style>

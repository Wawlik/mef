<template>
	<div class="inputGroup">
		<input type="text" :id="'input-' + data.id"
			v-model="count"
			@click="$emit('focus-gained')"
			@blur="$emit('focus-lost')"
			style="width:50px;min-width:50px!important"
			@input="checkText($event.target.value, $event.target, $event)">
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
			count: this.$props.data.count || 0,
			//last value
			vl: null
		}
	},
	methods: {
		//decorator case
		// input_inputHandler(target){
		// 	// count is last value
		// 	const isEqual = target.value === this.count;
		// 	const InputFormatType = {
		// 		THOUSAND: 3
		// 	};
		// 	const decorator = ( value, format = InputFormatType.THOUSAND ) => value
		// 		.replace(/[^\d]/g,'')
		// 		.split( '' )
		// 		.filter( char => char !== " " )
		// 		.reverse()
		// 		.reduce( ( result, char, index ) => result += (( index >= format && index % format === 0 ? " " : "" ) + +char), "" )
		// 		.split( '' )
		// 		.reverse()
		// 		.join( '' );
		// 		this.count = decorator( target.value );
		// 		// to save caret position
		// 		setTimeout(() => {
		// 			target.selectionStart = target.selectionEnd = this.carPos;
		// 		}, 1);
		// 		//remember last value to compare whenever its deleting or not
		// 	this.autoSizeInput(this.count);
		// },
		replaceSpaces(v){
			// '\D' === not digit
			return v.replace(/\D/g, '');
		},
		checkText(val = this.$props.data.count, target, evt) {
			console.log({evt})
			console.log({target})
            if(typeof val !== 'number') {
				// replace all spaces
				val = this.replaceSpaces(val);
			}

			let v = null;

			if(Number.isNaN(+val)) {
				// replace all letters
				v = +val.replace(/[^\d]/g,'');
			}
			v = v || val;

			// case deleting ' '
			let vtls = (+v).toLocaleString();
			const tss = target?.selectionStart;
			if(evt?.inputType === 'deleteContentBackward' && (vtls === this.vl)){
				// backspace btn
				vtls = vtls.slice(0, tss-1) + vtls.slice(tss, vtls.length);
				setTimeout(() => {
					target.selectionStart = target.selectionEnd = tss - 1;
				}, 1);
				vtls = this.replaceSpaces(vtls);
				vtls = (+vtls).toLocaleString();
			} else if (evt?.inputType === 'deleteContentForward' && (vtls === this.vl)){
				// delete btn 
				vtls = vtls.slice(0, tss+1) + vtls.slice(tss+2, vtls.length);
				setTimeout(() => {
					target.selectionStart = target.selectionEnd = tss+1;
				}, 1);
				vtls = this.replaceSpaces(vtls);
				vtls = (+vtls).toLocaleString();
			} else if (evt?.inputType === 'deleteContentForward'){
				setTimeout(() => {
					target.selectionStart = target.selectionEnd = tss;
				}, 1);
			} else if (evt?.inputType === 'deleteContentBackward'){
				setTimeout(() => {
					target.selectionStart = target.selectionEnd = tss;
				}, 1);
			} else if(evt?.data?.length){
				target.selectionStart = target.selectionEnd = tss;
			}

			// if(!Number.isNaN(+v)) {
			this.count = vtls;
			this.autoSizeInput(this.count);
			// }
			//save last state to get caret pos if needed
			this.vl = this.count;
		},
        autoSizeInput(count) {
			const input = document.getElementById('input-'+this.$props.data.id);
            if(input) {
				// const userAgent = navigator.userAgent.toLowerCase();
				// const Mozila = /firefox/.test(userAgent);
				input.style.width = ((count.length + 1) * 6.7) + 'px';
            }
        }
	},
	computed:{
		numData(){
			return +this.replaceSpaces(this.count);
		}
	},
    mounted(){
        this.checkText();
    }
}
</script>

<style scoped>
/* scoped to disable outer interactions of styles */
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
		font-family: 'Roboto', sans-serif;
	}
	.inputGroup > input:focus{
		border: none;
		outline: none;
		border-bottom: 1px solid #2540ff;
		caret-color: #2540ff;
	}
</style>

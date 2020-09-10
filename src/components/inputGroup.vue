<template>
	<div class="inputGroup">
		<input type="text" :id="'input-' + data.id"
			v-model="count"
			@click="$emit('focus-gained')"
			@blur="$emit('focus-lost')"
			style="width:50px;min-width:50px!important"
			@input="getFormattedText($event.target.value, $event.target, $event)">
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
		getFormattedText(val = this.$props.data.count, target, evt) {
			// console.log({evt})
            // if(typeof val !== 'number') {
			// 	// replace all spaces
			// 	val = this.replaceSpaces(val);
			// }

			let v = null;

			if(Number.isNaN(+val)) {
				// replace all letters
				v = +val.replace(/[\D]/g,'');
			}
			v = v || val;

			// case deleting ' '
			let vtls = (+v).toLocaleString();
			const tss = target?.selectionStart;
			if(evt?.inputType === 'deleteContentBackward' && (vtls === this.vl)){
				// backspace btn
				vtls = this.deleteContentBackward(vtls, target, tss);
				this.setCaretPos(target, tss-1);
			} else if (evt?.inputType === 'deleteContentForward' && (vtls === this.vl)){
				// delete btn 
				vtls = this.deleteContentForward(vtls, target, tss);
				this.setCaretPos(target, tss+1);
			}  else if (evt?.inputType === 'deleteContentBackward' || evt?.inputType === 'deleteContentForward'){
				this.setCaretPos(target, tss);
			} else if(evt?.inputType === 'insertText'){
				// insert text save caret pos
				if(tss !== vtls.length){
					//get without spaces length
					const lvs = this.replaceSpaces(vtls);
					//if next num adding new space
					if(lvs.length % 3 == 1){
						this.setCaretPos(target, tss+1);
					} else {
						this.setCaretPos(target, tss);
					}
				}
			}

			// if(!Number.isNaN(+v)) {
			this.count = vtls;
			this.autoSizeInput(this.count);
			// }
			//save last state to get caret pos if needed
			this.vl = this.count;
		},
		replaceSpaces(v){
			// '\D' === not digit
			return v.replace(/\D/g, '');
		},
		deleteContentBackward(v, target, tss){
			v = v.slice(0, tss-1) + v.slice(tss, v.length);
			return this.getFormatedInputString(v);
		},
		deleteContentForward(v, target, tss){
			v = v.slice(0, tss+1) + v.slice(tss+2, v.length);
			return this.getFormatedInputString(v);
		},
		getFormatedInputString(v){
			v = this.replaceSpaces(v);
			return (+v).toLocaleString();
		},
		setCaretPos(target, t){
			setTimeout(() => {
				target.selectionStart = target.selectionEnd = t;
			}, 1);
		},
        autoSizeInput(count) {
			const input = document.getElementById('input-'+this.$props.data.id);
            if(input) {
				// const userAgent = navigator.userAgent.toLowerCase();
				// const Mozila = /firefox/.test(userAgent);
				input.style.width = ((count.length + 1) * 6.7) + 'px';
            }
        },
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
		// 	this.autoSizeInput(this.count);
		// },
	},
	computed:{
		inputData(){
			return +this.replaceSpaces(this.count);
		}
	},
    mounted(){
        this.getFormattedText();
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

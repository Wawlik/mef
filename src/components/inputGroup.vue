<template>
	<div class="inputGroup">
		<div :class="'person ' + (focused ? 'person--colored' :'')">
			<div class="person-avatar--wrapper">
				<img src="@/assets/hugh.png" alt="" class="person-avatar">
			</div>
			<div class="person-info">
				<span class="person-info-name">{{data.name.toUpperCase()}} IS</span>
				<div class="person-info-input-group">
					<input class="person-count--input" type="text" :id="'input-' + data.id"
						v-model="count"
						@click="focused = true"
						@blur="focused = false"
						style="width:50px;min-width:50px!important"
						placeholder="7"
						@input="getFormattedText($event.target.value, $event.target, $event)">
					<span class="person-info-input-prefix">{{data.prefix}}</span>
				</div>
			</div>
		</div>
	</div>
</template>

<script>

export default {
	name: 'inputGroup',
	props: {
		data: {
			type: Object,
			required: true
		}
	},
	data(){
		return{
			count: this.$props.data?.count || '',
			// last value
			vl: null,
			focused: null
		}
	},
	methods: {
		getFormattedText(val = this.$props.data.count, target, evt) {
			// console.log({evt})
            if(typeof val !== 'number') {
				// replace all spaces
				val = this.replaceSpaces(val);
			}

			let v = null;

			if(Number.isNaN(+val)) {
				// replace all letters
				v = +val.replace(/[\D]/g,'');
			}
			v = v || val;

			// case deleting ' '
			let vtls = (v? +v : '').toLocaleString();
			vtls = vtls.replace(/[,]/g, ' ');
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
			} else if(evt?.inputType === 'insertText' && evt?.data){
				console.log({evt})
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
			this.count = vtls || '';
			this.autoSizeInput(this.count);
			// }
			// save last state to get caret pos if needed
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
			// replace commas
			return ((v? +v : '').toLocaleString()).replace(/[,]/g,' ');
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

<style >
/* scoped to disable outer interactions of styles */
	.person{
		display: flex;
		margin-bottom: 20px;
		justify-content: flex-start;
		align-items: center;
		color: #2c2c30;
		font-family: 'Roboto', sans-serif;
	}
	.person-avatar--wrapper{
		display: inline-block;
		border-radius: 50%;
		width: 104px;
		height: 104px;
	}
	.person-avatar{
		border-radius: 50%;
		width: 100px;
		height: 100px;
		padding-left: 4px;
	}
	.person-info{
		margin-left: 20px;
		width: 100%;
		text-align: left;
	}
	.person-info-input-group{
		display: flex;
		margin-top: 20px;
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
	.person-count--input{
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
	.person-count--input:focus{
		border: none;
		outline: none;
		border-bottom: 1px solid #2540ff;
		caret-color: #2540ff;
	}
</style>

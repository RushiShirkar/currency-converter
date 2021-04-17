<template>
	<div class="container align-items-center">
		<b-card class="mt-5">
			<div class="text-center">
				<h2>Currency Converter</h2>
			</div>
			<div class="text-center mt-5">
				<p>1 Indian Rupee equals</p>
				<p>{{currencyValueToRupee}} {{selectedCurrency}}</p>
			</div>
			<div class="row mt-4">
				<div class="col">
					<b-form-input 
		 				@input="calculateA_B" 
		 				v-model="calc1"
		 			>
		 			</b-form-input>
				</div>
				<div class="col">
					<b-form-select 
		 				v-model="selectedCurrency" 
		 				:options="currencies" 
		 				value-field="currency" 
		 				text-field="currency"
		 				@change="updateCurrency"
		 			>
		 			</b-form-select>
				</div>
			</div>
			<div class="row mt-4">
				<div class="col">
					<b-form-input
						v-model="calc2" 
						@input="calculateB_A"
					>
					</b-form-input>
				</div>
				<div class="col">
					<b-form-input
		 				placeholder="Indian Rupee"
		 			>
		 			</b-form-input>
				</div>
			</div>
		</b-card>
	</div>
</template>

<script>
export default {
	name: 'Home',
    data() {
	  	return{
	  		selectedCurrency: "Euro",
			currencyRate: "84.39",
			selectedInput1: false, 
			currencyValueToRupee: null,
			calc1: "",
			calc2: "",
			currencies: [
				{
					currency: "Euro",
					rate: 84.39,
				},
				{
					currency: "USD",
					rate: 73.32
				},
				{
					currency: "GBP",
					rate: 96.52
				},
				{
					currency: "AUD",
					rate: 60.52
				},
				{
					currency: "CAD",
					rate: 57.31
				},
				{
					currency: "YEN",
					rate: 0.67
				}
			]
	  	}
	},
	mounted() {
		this.currencyValueToRupee = ( 1 / this.currencyRate).toFixed(3);
	},
	methods: {
		updateCurrency(){
			var selected, i;
			this.currencyValueToRupee = ( 1 / this.currencyRate).toFixed(3);
			for(i = 0; i < this.currencies.length; i++){
				if(this.selectedCurrency == this.currencies[i].currency){
					selected = this.currencies[i];
				}
			}
			this.currencyRate = selected.rate;
			if(this.selectedInput1){ 
				if(isNaN(this.calc2)){
					this.calc2 = "";
					this.calc1 = "";
					return;
				}
				this.calc1 = (this.calc2 * this.currencyRate).toFixed(3);
			}
			else {
				if(isNaN(this.calc2)){
					this.calc2 = "";
					this.calc1 = "";
					return;
				}
				this.calc1 = (this.calc2 / this.currencyRate).toFixed(3);
			}
			if(this.calc1 == 0 || this.calc2 == 0) {
				this.calc2 = "";
				this.calc1 = "";
			}
		},
		calculate(value) {
			if(isNaN(value)){
				this.calc2 = "";
				this.calc1 = "";
				return;
			}
			if(this.selectedInput1){
				this.calc1 = value;
				this.calc2 = (value * this.currencyRate).toFixed(3);
			} else {
				this.calc2 = value;
				this.calc1 = (value / this.currencyRate).toFixed(3);
			}
			if(this.calc1 == 0 || this.calc2 == 0) {
				this.calc2 = "";
				this.calc1 = "";
			}
		},
		calculateA_B(){
			this.selectedInput1 = true;
			this.calculate(this.calc1);
		},
		calculateB_A(){
			this.selectedInput1 = false;
			this.calculate(this.calc2);
		}
	}
}
</script>

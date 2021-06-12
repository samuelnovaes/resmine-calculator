<template>
	<div>
		<label for="ghsInicial">GH/s inicial</label>
		<br>
		<input type="number" min="0" v-model="ghsInicial" id="ghsInicial" @input="calculate">
		<br>
		<br>
		<label for="income">Tipo de ciclo</label>
		<br>
		<select v-model="income" @change="calculate">
			<option v-for="(type, i) in cycleTypes" :key="i" :value="type.income">{{type.label}}</option>
		</select>
		<br>
		<br>
		<label for="cycles">Ciclos</label>
		<br>
		<input type="number" min="1" step="1" v-model="cycles" id="cycles" @input="calculate">
		<p>GH/s final: {{ghsFinal.toFixed(8)}}</p>
	</div>
</template>

<script>
export default {
	data() {
		return {
			ghsInicial: 100,
			cycles: 0,
			income: 2.3256,
			ghsFinal: 0,
			cycleTypes: [
				{ label: 'Por hora', income: 0.0969 },
				{ label: 'Por dia', income: 2.3256 },
				{ label: 'Por semana', income: 16.2791 },
				{ label: 'Por mês', income: 69.7674 },
				{ label: 'A cada 3 meses', income: 209.3023 },
				{ label: 'Por ano', income: 848.8372 }
			]
		}
	},
	mounted() {
		this.calculate();
	},
	methods: {
		calculate() {
			this.ghsFinal = Number(this.ghsInicial);
			for (let i = 0; i < this.cycles; i++) {
				this.ghsFinal += this.ghsFinal * (this.income / 100);
			}
		}
	}
}
</script>
<template>
	<dialog open>
		<label for="ghsInicial">GH/s inicial</label>
		<br>
		<input type="number" min="0" v-model="ghsInicial" id="ghsInicial">
		<br>
		<br>
		<label for="income">Tipo de ciclo</label>
		<br>
		<select v-model="income">
			<option v-for="(type, i) in cycleTypes" :key="i" :value="type.income">{{type.label}}</option>
		</select>
		<br>
		<br>
		<label for="cycles">Ciclos</label>
		<br>
		<input type="number" min="1" step="1" v-model="cycles" id="cycles">
		<br>
		<br>
		<button @click="calculateGhs">Calcular GH/s final</button>
		<br>
		<br>
		<label for="ghsFinal">GH/s final</label>
		<br>
		<input type="number" :min="ghsInicial" v-model="ghsFinal" id="ghsFinal">
		<br>
		<br>
		<button @click="calculateCycles">Calcular ciclos</button>
	</dialog>
</template>

<style scoped>
* {
	font-family: Arial, Helvetica, sans-serif;
}
input, select, button {
	padding: 5px;
	width: 200px;
	box-sizing: border-box;
}
dialog {
	position: absolute;
	top: 50%;
	transform: translateY(-50%);
	border: none;
}
</style>

<script>
export default {
	data() {
		return {
			ghsInicial: '100.00000000',
			cycles: '1',
			income: 2.3256,
			ghsFinal: '100.00000000',
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
		this.calculateGhs();
	},
	methods: {
		calculateGhs() {
			this.ghsFinal = Number(this.ghsInicial);
			for (let i = 0; i < this.cycles; i++) {
				this.ghsFinal += this.ghsFinal * (this.income / 100);
			}
			this.ghsFinal = this.ghsFinal.toFixed(8);
		},
		calculateCycles() {
			let ghsFinal = Number(this.ghsInicial);
			this.cycles = 0;
			while(ghsFinal < this.ghsFinal) {
				ghsFinal += ghsFinal * (this.income / 100);
				this.cycles++;
			}
		}
	}
}
</script>
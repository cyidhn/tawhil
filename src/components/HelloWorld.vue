<template>
	<v-container>
		<v-row class="text-center">
			<v-col cols="12" v-if="!step2">
				<h2 class="mb-5 headline font-weight-bold mb-3">
					Sélectionner un fichier CSV
				</h2>
				<v-file-input
					accept=".txt"
					label="Importer un fichier texte"
					color="black"
					outlined
					v-model="csvFile"
				></v-file-input>
				<v-btn block color="primary" @click="analyseCSV" v-if="csvFile">
					Étape suivante
				</v-btn>
				<v-btn block color="primary" disabled v-else>
					Étape suivante
				</v-btn>
			</v-col>
			<v-col cols="12" v-if="step2">
				<h2 class="mb-5 headline font-weight-bold mb-3">
					Sélectionner les paramètres (*)
				</h2>
				<div v-for="check in checkbox" :key="check.id">
					<v-checkbox
						class="mb-1 mt-1 pt-1 pb-1"
						v-model="check.coche"
						:label="check.name"
					></v-checkbox>
				</div>
			</v-col>
		</v-row>
	</v-container>
</template>

<script>
	export default {
		name: "HelloWorld",

		data: () => ({
			csvFile: null,
			result: null,
			data: null,
			step2: false,
			step3: false,
			checkbox: [],
			count: 0,
		}),

		methods: {
			analyseCSV() {
				let resultsdata;

				this.$papa.parse(this.csvFile, {
					complete: function(results) {
						resultsdata = results;
						console.log(results);
					},
				});

				setTimeout(() => {
					this.result = resultsdata;
					this.data = this.result.data[0];
					let checking = [];
					for (let i = 0; i < this.data.length; i++) {
						checking.push({ id: i, name: this.data[i], coche: false });
					}
					this.checkbox = checking;
					this.step2 = true;
				}, 2000);
			},
		},
	};
</script>

<template>
	<v-container>
		<v-row class="text-left">
			<v-col cols="12" v-if="step1">
				<div class="mb-12 mt-8">
					<center>
						<v-img
							:src="require('../assets/Tawhil_header.png')"
							width="750"
							class="ct-img"
						></v-img>
					</center>
				</div>
				<h2 class="mb-5 pt-12 mt-12 headline font-weight-bold mb-3">
					Select a CSV file
				</h2>
				<v-file-input
					accept=".txt"
					label="Importer un fichier texte"
					color="black"
					outlined
					v-model="csvFile"
				></v-file-input>
				<v-btn block color="#FF904D" dark @click="analyseCSV" v-if="csvFile">
					Next step
				</v-btn>
				<v-btn block color="#FF904D" disabled v-else>
					Next step
				</v-btn>
			</v-col>
			<v-col cols="12" v-if="step2">
				<h2 class="mb-5 headline font-weight-bold mb-3">
					Select the parameters (*) to keep
				</h2>
				<div v-for="check in checkbox" :key="check.id">
					<v-checkbox
						class="mb-1 mt-1 pt-1 pb-1 mt-0 mb-0"
						color="#FF904D"
						v-model="check.coche"
						:label="check.name"
					></v-checkbox>
				</div>
				<v-btn block color="#FF904D" dark @click="getResults1" class="mt-6">
					Next step
				</v-btn>
			</v-col>
		</v-row>
	</v-container>
</template>

<script>
	export default {
		data: () => ({
			csvFile: null,
			result: null,
			data: null,
			step1: true,
			step2: false,
			step3: false,
			checkbox: [],
			count: 0,
		}),

		methods: {
			getResults1() {
				const getCoche = this.checkbox.filter((x) => x.coche).map((x) => x.id);

				console.log(getCoche);

				if (!getCoche.length) {
					window.alert(
						"Error, you must select at least one item for analysis."
					);
				} else {
					this.step2 = false;
					this.step3 = true;
				}
			},
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
					this.step1 = false;
				}, 2000);
			},
		},
	};
</script>

<style scoped>
	.ct-img {
		border: 5px solid black;
	}
</style>

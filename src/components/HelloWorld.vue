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
					label="Your CSV file"
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
			<v-col cols="12" v-if="step3">
				<h2 class="mb-5 headline font-weight-bold mb-3">
					Select main text (only one field to select only)
				</h2>
				<div v-for="check in checkbox" :key="check.id">
					<v-checkbox
						class="mb-1 mt-1 pt-1 pb-1 mt-0 mb-0"
						color="#FF904D"
						v-model="check.main"
						@click="getResults2(check.id)"
						:label="check.name"
					></v-checkbox>
				</div>
			</v-col>
			<v-col cols="12" v-if="step4">
				<h2 class="mb-5 headline font-weight-bold mb-3">
					Your file is almost ready. In what format do you want to extract it?
				</h2>
				<div>
					<v-btn
						depressed
						class="mr-3"
						color="#FF904D"
						dark
						@click="getIramuteq"
					>
						Export for Iramuteq
					</v-btn>
					<v-btn
						depressed
						class="mr-3"
						color="#FF904D"
						disabled
						@click="getIramuteq"
					>
						Export in JSON (soon)
					</v-btn>
					<v-btn
						depressed
						class="mr-3"
						color="#FF904D"
						disabled
						@click="getIramuteq"
					>
						Export in XML (soon)
					</v-btn>
				</div>
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
			step4: false,
			checkbox: [],
			idToTextExport: null,
			count: 0,
		}),

		methods: {
			getIramuteq() {
				// Get elements
				const getCocheStars = this.checkbox
					.filter((x) => x.coche)
					.map((x) => x.id);

				const getCocheMain = this.checkbox
					.filter((x) => x.main)
					.map((x) => x.id);

				const getData = this.result.data;

				// Get text
				let text = "";

				// Stars & text
				for (let i = 0; i < getData.length; i++) {
					// Element
					let e1 = getData[i];

					// Stars
					for (let l = 0; l < getCocheStars; l++) {
						let traitement = e1[getCocheStars[l]];
						traitement = traitement.replace(" ", "");
						traitement = traitement.replace("*", "x");
						traitement = traitement.replace("'", "");
						traitement = traitement.replace('"', "");
						traitement = traitement.replace(",", "");
						traitement = traitement.replace(";", "");
						text += `*{${traitement}} `;
					}

					// Espacement
					text += "\n";

					// Text
					let mainText = e1[getCocheMain[0]];
					mainText = mainText.replace("*", "x");
					text += mainText;

					// Espacement
					text += "\n\n";
				}

				this.exp(text, "export.txt");
			},
			exp(text, file) {
				let data = new Blob([text], { type: "text/plain" });
				let link = document.createElement("a");
				link.href = window.URL.createObjectURL(data);
				link.download = file;
				link.click();
			},
			getResults2(theID) {
				console.log(theID);
				this.idToTextExport = theID;
				this.step4 = true;
				this.step3 = false;
			},
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
						checking.push({
							id: i,
							name: this.data[i],
							coche: false,
							main: false,
						});
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

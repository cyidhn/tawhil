<template>
	<v-container>
		<v-row class="text-center">
			<v-col class="mt-5" cols="12">
				<h2 class="headline font-weight-bold mb-3">
					Sélectionner un fichier CSV
				</h2>
			</v-col>
			<v-col cols="12">
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
		</v-row>
	</v-container>
</template>

<script>
	export default {
		name: "HelloWorld",

		data: () => ({
			csvFile: null,
		}),

		methods: {
			analyseCSV() {},
		},
	};
</script>

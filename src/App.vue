<template>
	<v-app>
		<v-app-bar app color="#FF904D" dark>
			<div class="d-flex align-center">
				<a href="/">
					<v-img :src="require('./assets/Tawhil.png')" width="150"></v-img>
				</a>
			</div>

			<v-spacer></v-spacer>

			<v-btn
				href="https://github.com/demangejeremy/csv-to-iramuteq"
				target="_blank"
				text
			>
				<span class="mr-2">See on GitHub</span>
				<v-icon>mdi-open-in-new</v-icon>
			</v-btn>
		</v-app-bar>

		<v-main>
			<HelloWorld />
		</v-main>

		<v-footer class="font-weight-medium mt-12">
			<v-col class="text-center" cols="12">
				{{ new Date().getFullYear() }} —
				<strong>Jérémy DEMANGE | CY IDHN</strong>
			</v-col>
		</v-footer>
	</v-app>
</template>

<script>
	import HelloWorld from "./components/HelloWorld";

	export default {
		name: "App",

		components: {
			HelloWorld,
		},

		data: () => ({
			//
		}),
	};
</script>

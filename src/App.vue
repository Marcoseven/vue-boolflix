<template>
	<div id="app">
		<SiteHeader />
		<div class="container">
			<div class="row mt-5 text-center">
				<form>
					<label for="movie"></label>
					<input
						class="p-2 text-left"
						type="text"
						name="movie"
						id="movie"
						placeholder="Inserire nome film"
						v-model="movieUserInput"
					/>
					<button class="w-auto ms-3 p-2 border-primary" @click="callApi">
						Inizia >
					</button>
				</form>
				<div class="movies">
					<div class="movie" v-for="movie in movies" :key="movie.id">
						<h3>Titolo: {{ movie.title.toUpperCase() }}</h3>
						<h4>Titolo originale: {{ movie.original_title }}</h4>
						<h4>Lingua originale: {{ movie.original_language }}</h4>
						<h4>Voto: {{ movie.vote_count }}</h4>
					</div>
				</div>
			</div>
		</div>

		<SiteMain />
		<SiteFooter />
	</div>
</template>

<script>
import axios from "axios";

import SiteHeader from "./components/SiteHeader";
import SiteMain from "./components/SiteMain";
import SiteFooter from "./components/SiteFooter";

export default {
	name: "App",
	components: {
		SiteHeader,
		SiteMain,
		SiteFooter,
	},
	data() {
		return {
			movieUserInput: "",
			movies_url: "https://api.themoviedb.org/3/search/movie",
			api_key: "5eba0868299db17ce62c13e86462a76c",
			error: null,
			movies: "",
		};
	},
	methods: {
		callApi() {
			const urlCompiled = `${this.movies_url}?api_key=${this.api_key}&query=${this.movieUserInput}`;
			axios
				.get(urlCompiled)
				.then((resp) => {
					console.log(resp);
					this.movies = resp.data.response;
				})
				.catch((err) => {
					console.log("OPS! C'è un errore: ", err);
					this.error = err;
				});
		},
	},
};
</script>

<style lang="scss">
@import "../node_modules/bootstrap/scss/bootstrap.scss";
@import "./assets/scss/common.scss";
@import "./assets/scss/variables.scss";
</style>

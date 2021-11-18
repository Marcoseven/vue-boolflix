<template>
	<div id="SiteMain">
		<div class="container">
			<h2 class="text-center">Film, serie TV e tanto altro. Senza limiti.</h2>
			<div class="row">
				<SearchMovie @search-movie="search" />
				<div class="col-2" v-for="movie in films" :key="movie.id">
					<div class="movie text-center">
						<h3>Titolo: {{ movie.title.toUpperCase() }}</h3>
						<div>
							<h4>Titolo originale: {{ movie.original_title }}</h4>
							<h4>Lingua originale: {{ movie.original_language }}</h4>
							<h4>Voto: {{ movie.vote_count }}</h4>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import SearchMovie from "./SearchMovie.vue";
import axios from "axios";

export default {
	components: {
		SearchMovie,
	},
	data() {
		return {
			movies: "",
			loading: true,
			error: "",
			api_key: "5eba0868299db17ce62c13e86462a76c",
			queryUserInput: "",
		};
	},
	mounted() {
		setTimeout(this.callApi, 1000);
	},
	methods: {
		callApi() {
			axios
				.get(
					"https://api.themoviedb.org/3/search/movie?api_key=" +
						this.api_key +
						"&language=en-US&query=" +
						this.queryUserInput +
						"&page=1&include_adult=false"
				)
				.then((r) => {
					console.log(r.data);
					this.movies = r.data.response;
					this.loading = false;
				})
				.catch((e) => {
					console.log(e, "OPS!");
					this.error = `'OPS!' ${e}`;
				});
		},
	},
	computed: {
	},
};
</script>

<style lang="scss">
@import "../assets/scss/variables.scss";
@import "../assets/scss/common.scss";
</style>

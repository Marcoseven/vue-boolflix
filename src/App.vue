<template>
	<div id="app">
		<SiteHeader />
		<div class="container">
			<div class="row mt-5">
				<form class="mb-5 text-center">
					<label for="movie"></label>
					<input
						class="p-2 text-left w-25 border-primary"
						type="text"
						name="movie"
						id="movie"
						placeholder="Inserire nome film o serie-TV"
						v-model="queryInput"
					/>
					<button
						class="w-auto ms-3 p-2 border-primary"
						@click.prevent="callApi"
					>
						Inizia
					</button>
				</form>
			</div>
			<div class="row">
				<div class="card" v-for="(data, key, index) in results" :key="index">
					<div class="col-3" v-for="result in data.results" :key="result.id">
						<div v-if="key === 'movies'">
							<h4>Titolo film: {{ result.title }}</h4>
							<h4>Titolo originario del film: {{ result.original_title }}</h4>
						</div>
						<div v-else>
							<h4>Titolo serie-tv: {{ result.name }}</h4>
							<h4>{{ result.original_name }}</h4>
						</div>
						<div>
							<h4 v-if="getFlags(original_language)">
								<country-flag
									:country="
										result.original_language === 'it'
											? 'it'
											: result.original_language
									"
									size="normal"
								/>
							</h4>
							<h4 v-else>
								Lingua originale del film: {{ result.original_language }}>
							</h4>
							<h4>
								Voti ottenuti:
								<span
									v-for="vote in parseInt((result.vote_average / 2))"
									:key="vote.id"
								>
									<i class="fa fa-star"></i>
								</span>
								<span
									v-for="vote in 5 - (result.vote_average / 2)"
									:key="vote.id"
								>
									<i class="fa fa-star"></i>
								</span>
							</h4>
							<h4>Overview: {{ result.overview }}</h4>
							<div class="image">
								<h4>Copertina:</h4>
								<img
									:src="`https://image.tmdb.org/t/p/w342/` + result.poster_path"
									alt=""
								/>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
		<SiteMain />
	</div>
</template>

<script>
import SiteHeader from "./components/SiteHeader";
import SiteMain from "./components/SiteMain";
import axios from "axios";

export default {
	name: "App",
	components: {
		SiteHeader,
		SiteMain,
	},
	data() {
		return {
			error: null,
			queryInput: "",
			api_key: "5eba0868299db17ce62c13e86462a76c",
			movies_url: "https://api.themoviedb.org/3/search/movie",
			series_url: "https://api.themoviedb.org/3/search/tv",
			results: {},
			flags: ["it", "fr", "es", "en", "nl"],
		};
	},
	methods: {
		callApi() {
			Promise.all([this.axiosMovies(), this.axiosSeries()])
				.then((resp) => {
					console.log(resp);
					const movies = resp[0].data;
					const series = resp[1].data;
					this.$set(this.results, "movies", movies);
					this.$set(this.results, "series", series);
				})
				.catch((err) => {
					console.log("OPS! C'è un errore: ", err);
					this.error = err;
				});
		},
		axiosMovies() {
			const urlCompiledFilms = `${this.movies_url}?api_key=${this.api_key}&query=${this.movieUserInput}`;
			return axios.get(urlCompiledFilms);
		},
		axiosSeries() {
			const urlCompiledSeries = `${this.movies_url}?api_key=${this.api_key}&query=${this.movieUserInput}`;
			return axios.get(urlCompiledSeries);
		},
		getFlags(flag) {
			return this.flags.includes(flag);
		},
	},
};
</script>

<style lang="scss">
@import "../node_modules/bootstrap/scss/bootstrap.scss";
@import "./assets/scss/common.scss";
@import "./assets/scss/variables.scss";

.container {
	overflow-x: auto;
}

.row {
	background-color: #000;
}

.card {
	width: calc(100% / 5);
	background-color: #000;
	.col-3 {
		height: auto;
		width: 100%;
		border: 1px solid #fff;
		margin: 0 0.5rem;
		padding: 1rem;
		background-color: #000;
		color: #fff;
		.content {
			height: 100%;
		}
	}
}

h4 {
	font-size: 15px;
	color: #fff;
}

.image img {
	width: 50%;
}
</style>

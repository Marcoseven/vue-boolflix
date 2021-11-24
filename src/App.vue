<template>
	<div id="app">
		<SiteHeader />
		<div class="container-input">
			<div class="row mt-5">
				<form class="mb-5 text-center">
					<label for="movie"></label>
					<input
						class="p-2 text-left w-25 border-primary"
						type="text"
						name="movie"
						id="movie"
						placeholder="Inserire nome film o serie-tv"
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
		</div>
		<div class="container-card">
			<div
				class="row justify-content-center flex-wrap"
				v-for="(data, key, index) in results"
				:key="index"
				:class="key"
			>
				<div class="card" v-for="result in data.results" :key="result.id">
					<div v-if="key === 'movies'">
						<h4>Titolo film: {{ result.title }}</h4>

						<h4>Titolo originario del film: {{ result.original_title }}</h4>
					</div>
					<div v-else>
						<h4>Titolo serie-tv: {{ result.name }}</h4>
						<h4>Titolo originario della serie-tv:{{ result.original_name }}</h4>
					</div>
					<div>
						<i class="fas fa-house-user"></i>
					</div>
					<div>
						<span v-if="Flags(result.original_language)">
							<span>
								<country-flag
									:country="
										result.original_language === 'en'
											? 'gb'
											: result.original_language
									"
									size="normal"
								/>
							</span>
						</span>
						<span v-else
							>Lingua originaria del film:
							{{ result.original_language }}
						</span>
					</div>
					<div>
						<div class="vote d-flex align-items-baseline">
							<h4 class="pe-3">Voti ottenuti:</h4>
							<span
								v-for="vote in Math.round(result.vote_average / 2).toFixed(0)"
								:key="vote.id"
							>
								<i class="fas fa-star"></i>
								<span
									v-for="vote in 5 -
									Math.round(result.vote_average / 2).toFixed(0)"
									:key="vote.id"
								>
									<i class="fas fa-star"></i
								></span>
							</span>
						</div>
						<h4>Overview: {{ result.overview }}</h4>
						<div class="image">
							<img
								:src="`https://image.tmdb.org/t/p/w342/` + result.poster_path"
								alt="immagine copertina di film o serie-tv"
							/>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import SiteHeader from "./components/SiteHeader";
import axios from "axios";

export default {
	name: "App",
	components: {
		SiteHeader,
	},
	data() {
		return {
			error: null,
			queryInput: "",
			api_key: "5eba0868299db17ce62c13e86462a76c",
			movies_url: "https://api.themoviedb.org/3/search/movie",
			series_url: "https://api.themoviedb.org/3/search/tv",
			results: {},
			flags: ["it", "fr", "es", "en", "nl", "da"],
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
			const urlCompiledFilms = `${this.movies_url}?api_key=${this.api_key}&query=${this.queryInput}`;
			return axios.get(urlCompiledFilms);
		},
		axiosSeries() {
			const urlCompiledSeries = `${this.series_url}?api_key=${this.api_key}&query=${this.queryInput}`;
			return axios.get(urlCompiledSeries);
		},
		Flags(iconFlags) {
			return this.flags.includes(iconFlags);
		},
	},
};
</script>

<style lang="scss">
@import "../node_modules/bootstrap/scss/bootstrap.scss";
@import "./assets/scss/common.scss";

.container {
	overflow-x: auto;
}

.row {
	padding: 0 0.5rem;
	margin: 0 -0.5rem;
	background-color: #000;
}

.card {
	width: calc(100% / 4);
	height: auto;
	display: flex;
	flex-direction: column;
	justify-content: flex-start;
	border: 1px solid #fff;
	margin: 0.5rem;
	padding: 0.5rem;
	background-color: #000;
	color: #fff;
}

h4 {
	padding: 0.3rem 0;
	margin: 0 !important;
	font-size: 15px;
	color: #fff;
}

.vote i {
	width: 10px;
	display: inline list-item;
	color: yellow;
}

.image img {
	width: 50%;
	padding: 0.3rem 0;
}
</style>

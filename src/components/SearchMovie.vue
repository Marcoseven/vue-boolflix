<template>
	<div class="searchMovie d-flex justify-content-center m-5">
		<form>
			<label for="movie"></label>
			<input
				class="p-2 text-left"
				type="text"
				name="movie"
				id="movie"
				placeholder="Inserire nome film"
				v-model="searchMovie"
			/>
			<button
				class="w-auto ms-3 p-2 border-primary"
				@click="$emit('search-movie', searchMovie)"
			>
				Inizia >
			</button>
		</form>
	</div>
</template>

<script>
export default {
	data() {
		return {
			searchMovie: "",
		};
	},
};
</script>

<style lang="scss">
button {
	border: none;
	color: #fff;
	background-color: red;
}
</style>

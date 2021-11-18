<template>
	<div id="SiteMain">
		<div class="container">
			<SearchBar />
		</div>
	</div>
</template>

<script>
import SearchBar from "./SearchBar.vue";

export default {
	components: {
		SearchBar,
	},
};
</script>

<style lang="scss">
@import "../assets/scss/variables.scss";
@import "../assets/scss/common.scss";
</style>

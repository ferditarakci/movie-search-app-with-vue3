<template>
	<div class="container py-5">
		<Header title="IMDb'de FİLM ARA" />
		<Search placeHolder="Aranacak kelime..." buttonText="Ara" :searchText="searchText" :totalResult="totalResult" @submit-data="searchEvent" />
		<Movies apiKey="API_KEY" ratingText="IMDb Puanı:" :searchText="searchText" :watchSearchText="searchText" @total-results="totalResultData" />
		<Footer copyright="Copyright 2021" link="https://www.ferditarakci.com" linkText="Created by Ferdi Tarakcı" />
	</div>
</template>

<script>
import Header from "./components/Header"
import Footer from "./components/Footer"
import Search from "./components/Search"
import Movies from "./components/Movies"

export default {
	name: "Movie Search App",
	components: {
		Header,
		Footer,
		Search,
		Movies
	},
	data() {
		return {
			searchText: "the matrix",
			totalResult: 0
		}
	},
	methods: {
		totalResultData(data) {
			this.totalResult = data
		},
		searchEvent(data) {
			this.searchText = data
		}
	}
}
</script>

<!-- SCSS formatındaki stil dosyamızı uygulamaya ekliyoruz. -->
<style lang="scss">
	@import "./assets/scss/style.scss";
</style>

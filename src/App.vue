<template>
	<div class="container py-5">
		<Header :title="title" />
		<Search
			placeHolder="Search word..."
			buttonText="Search"
			:searchText="searchText"
			:totalResult="totalResult"
			@submit-data="searchEvent"
		/>
		<Movies
			ratingText="IMDb Rating:"
			:apiKey="apiKey"
			:searchText="searchText"
			:watchSearchText="searchText"
			@total-results="totalResultData"
		/>
		<Footer
			copyright="Copyright 2021"
			link="https://www.ferditarakci.com.tr"
			linkText="Developed by Ferdi Tarakci"
		/>
	</div>
</template>

<script>
import Header from '@/components/Header'
import Footer from '@/components/Footer'
import Search from '@/components/Search'
import Movies from '@/components/Movies'

export default {
	name: 'Movie Search App',
	components: {
		Header,
		Footer,
		Search,
		Movies,
	},
	data() {
		return {
			title: process.env.VUE_APP_TITLE,
			apiKey: process.env.VUE_APP_API_KEY,
			searchText: 'the matrix',
			totalResult: 0,
		}
	},
	methods: {
		totalResultData(data) {
			this.totalResult = data
		},
		searchEvent(data) {
			this.searchText = data
		},
	},
}
</script>

<!-- SCSS formatındaki stil dosyamızı uygulamaya ekliyoruz. -->
<style lang="scss">
@import './assets/scss/style.scss';
</style>

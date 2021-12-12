<template>
	<section class="row movies">
		<div class="col-6 col-md-4 col-lg-3 mt-4" v-for="(item, index) in movies" :key="index">
			<div class="item">
				<a :href="getLink(item.imdbID)" target="_blank" class="image">
					<img :alt="item.Title" :src="item.Poster" loading="lazy">
					<span class="rating" :class="item.ratingClass">{{ item.imdbRatingText }}</span>
					<span class="year" :class="item.ratingClass" v-if="item.Year">Yıl {{ item.Year }}</span>
				</a>
				<h2><a :href="getLink(item.imdbID)" target="_blank">{{ item.Title }}</a></h2>
				<span class="rating-bar">
					<span :class="item.ratingClass"></span>
				</span>
			</div>
		</div>
		<nav class="col-12 mt-5" aria-label="Paginations" v-if="paginations">
			<ul class="pagination pagination-sm flex-wrap">
				<li class="page-item" v-for="(item, index) in paginations" :key="index" :class="{'active' : (currentPage == item)}">
					<a class="page-link" :href="'#' + item" @click="getPages(item)">{{ item }}</a>
				</li>
			</ul>
		</nav>
	</section>
</template>

<script>
import axios from "axios"

export default {
	name: "Movies",
	props: ["apiKey", "searchText", "watchSearchText", "ratingText"],
	data() {
		return {
			movies: [],
			totalResults: 0,
			paginations: null,
			currentPage: 1,
			apiUrl: "//www.omdbapi.com",
			apiParams: {
				apikey: this.apiKey,
				plot: "full",
				type: "movie",
				page: null,
				s: null
			}
		}
	},
	created() {
		// Sayfa ilk açılışta otomatik sorguyu çalıştıyoruz.
		this.fetchMovies()
	},
	watch: {
		// Arama kelimesinde değişiklik olup olmadığını dinliyoruz.
		watchSearchText() {
			this.fetchMovies();
		}
	},
	methods: {
		// Template içinde gereksiz kod kalabalığını azaltmak için link methodu.
		getLink(imdbID) {
			return "https://www.imdb.com/title/" + imdbID + "/"
		},

		// 1'den çok sayfa olması durumunda sayfalar arası geçiş metodumuz.
		getPages(page) {
			this.currentPage = page
			this.apiParams["page"] = page
			this.fetchMovies()
		},

		// Film datamızı oluşturacağımız metodumuzu oluşturuyoruz.
		async fetchMovies() {
			// console.log(this.searchText)

			// searchText değişkenimizdeki değeri API parametresine ekliyoruz.
			if (this.searchText) this.apiParams["s"] = this.searchText

			// Film datamızı çekmek için OMDb API'ya istek yapıyoruz.
			await axios.get(this.apiUrl, {params: this.apiParams})
			.then((response) => {
				// console.log( response )
	
				// Response'dan dönen search datasını this.movies'e atıyoruz.
				this.movies = response.data.Search || []

				// Response'dan dönen toplam sonuç sayısını this.totalResults'e atıyoruz.
				this.totalResults = parseInt(response.data.totalResults)
				this.paginations = Math.round(this.totalResults / 10)

				// console.log( this.totalResults )
				// Toplam sonucu $emit ile parent'a gönderiyoruz.
				this.$emit("total-results", this.totalResults)

				// Response'dan dönen datayı döngüye sokarak filmlerin puanlarını sorgulayıp objemize dahil ediyoruz.
				this.movies.forEach((item, index) => {

					// Film görselimiz "N/A" olarak dönüyorsa "NO IMAGE" görselini basıyoruz.
					if (this.movies[index]["Poster"] === "N/A") this.movies[index]["Poster"] = require("@/assets/images/no_image.jpg")

					this.fetchMovieRating(item.imdbID).then(response => {
						let imdbRating = response

						// Filmin puan durumu "N/A" olarak dönenerse "0" olarak değiştiriyoruz.
						if (imdbRating === "N/A") imdbRating = 0

						imdbRating = parseFloat(imdbRating)

						// "0 / 10" arasındaki puan bilgisini 10 ile çarparak 100 üzerinden hesaplama yaptırıyoruz.
						let ratingWidth = imdbRating * 10

						// Yeni puan "0 <> 20" arası kırmızı, "21 <> 50" arası sarı, 50 üzerinde ise yeşil renk ekliyoruz.
						let ratingClass = "bg-red"
						if (ratingWidth > 20 && ratingWidth <= 50) ratingClass = "bg-yellow"
						if (ratingWidth > 50) ratingClass = "bg-green"

						this.movies[index]["ratingClass"] = ratingClass
						this.movies[index]["imdbRating"] = imdbRating
						this.movies[index]["imdbRatingText"] = this.ratingText + " " + imdbRating + " / 10"
					})
				})
			})
			.catch(error => {
				console.error( error )
			})
		},

		// Filmlerin puanlarını sorgulamak için methodumuzu oluşturuyoruz.
		async fetchMovieRating(imdbID) {
			return await new Promise((resolve, reject) => {
				let response = axios.get(
									this.apiUrl, {
										params: {
											apikey: this.apiParams.apikey,
											i: imdbID
										}
									}).then(response => {return response.data.imdbRating})
				if (response) {
					resolve(response)
				} else {
					reject("error")
				}
			})
		}
	}
}
</script>
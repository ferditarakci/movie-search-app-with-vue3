<template>
	<section class="row align-items-center">
		<div class="col-12 col-md-6">
			<form class="d-flex search" @submit.prevent="searchForm">
				<div class="label" ref="label">
					<input
						:placeholder="placeHolder"
						:value="searchText"
						@mousedown="searchFocus"
						type="text"
						class="form-control"
						autocomplete="off"
						required
					/>
					<ul class="prev-search" ref="prevSearch">
						<li
							v-for="(item, index) in prevSearch"
							:key="index"
							@click="newSearch(item)"
						>
							{{ item }}
							<i class="remove" @click="deleteSearch(index)">
								&times;
							</i>
						</li>
					</ul>
				</div>
				<button type="submit" class="btn btn-success ml-2">
					{{ buttonText }}
				</button>
			</form>
		</div>
		<div class="col-12 col-md-6 mt-3 mt-md-0 text-md-right">
			<strong v-if="totalResult">Toplam Sonuç: {{ totalResult }}</strong>
			<strong class="red" v-else>Arama yapınız</strong>
		</div>
		<div class="col-12">
			<div class="border-top mt-4"></div>
		</div>
	</section>
</template>

<script>
export default {
	name: 'Search',
	props: ['placeHolder', 'searchText', 'buttonText', 'totalResult'],
	data() {
		return {
			prevSearch: [],
		}
	},
	created() {
		// Önceki aramalarımızı görmek ve hızlı arama yapmabilmek için localStorage'da kayıtlı verimizi alıp "autocomplete" listemizi dolduruyoruz.
		this.searchKeys('', 'prevlist')
	},
	mounted() {
		// Arama listemiz dışında bir alana tıklandığında listeyi kapatıyoruz.
		window.addEventListener('mousedown', (e) => {
			if (
				!(
					e.target.parentNode.className.indexOf('label') > -1 ||
					e.target.parentNode.className == 'prev-search'
				) &&
				e.target.className != 'remove'
			)
				this.$refs.label.classList.remove('focus')
		})
	},
	methods: {
		// Birden fazla metod içinden parent'a data göndereceğimiz için bu metodu oluşturuyoruz.
		submitData(val) {
			this.$emit('submit-data', val)
		},

		// Arama kutusuna tıklandığında "autocomplete" listemizi görünür hale getirmek için gerekli class'ını ekliyoruz.
		searchFocus() {
			this.$refs.label.classList.add('focus')
		},

		// Hızlı arama "autocomplete" listemizden daha önce aranmış bir kelimeye tıklayarak yeniden arama yapabilmeyi sağlıyoruz.
		newSearch(val) {
			this.submitData(val)
		},

		// Hızlı arama "autocomplete" listemizden daha önce aranmış bir kelimeyi siliyoruz.
		deleteSearch(index) {
			this.searchKeys(index, 'delete')
			this.prevSearch.splice(index, 1)
		},

		// Arama yapabilmek için formu submit ediyoruz.
		searchForm($event) {
			let val = $event.target[0].value
			this.submitData(val)
			this.searchKeys(val)
		},

		// Arama listesiyle ilgili işlemler için metodumuzu oluşturuyoruz.
		searchKeys(text, method = 'add') {
			if (text == '' && method == 'add') return false

			// Object'imizi tanımlıyoruz.
			let obj = localStorage.getItem('searchList') || {}

			// localStorage'da önceki aramalarımız varsa string'ten object'e parse ediyoruz.
			if (typeof obj === 'string') obj = JSON.parse(obj)

			// Object'mizi array'e dönüştürüyoruz.
			obj = Object.values(obj)

			// Daha önce aranana kelime yeniden aratıldığında dizi içindeki ilgili elemanı siliyoruz, böylelikle yeniden ekleyip ilk sıraya getiriyoruz.
			if (method == 'add' && obj.indexOf(text) > -1) {
				obj.splice(obj.indexOf(text), 1)
			}

			// Formdan gelen kelime dizi içinde yoksa ekliyoruz.
			// Aynı zamanda localStorage'daki eski aramalar 10'dan fazla ise en eski olan kaydı listeden siliyoruz.
			if (method == 'add' && obj.indexOf(text) == -1) {
				if (obj.length > 10) obj.splice(0, 1)
				obj.push(text)
			}

			// Dizi içindeki ilgili elemanı siliyoruz.
			else if (method == 'delete') {
				obj.splice(text, 1)
			}

			// Önceki aramaların "autocomplete" listesine eklenmesi için prevSearch array'ine ekliyoruz.
			else if (method == 'prevlist') {
				this.prevSearch = obj
			}

			// Arama listemizi localStorage'a json string olarak kaydediyoruz.
			localStorage.setItem(
				'searchList',
				JSON.stringify(Object.assign({}, obj)),
			)

			// Yeni arama yapıldığında "autocomplete" listemize anında eklenmesini sağlıyoruz.
			if (method == 'add') {
				this.searchKeys('', 'prevlist')
			}

			// "autocomplete" listemizi son arananan kelimeden ilk aranan kelimeye sıralıyoruz.
			if (obj.length) obj.reverse()
		},
	},
}
</script>

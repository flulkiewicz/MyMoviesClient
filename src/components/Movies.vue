<template>
	<div class="container bg-dark p-5 rounded text-orange">
		<h3 class="mb-3 border-bottom-orange pb-3 px-2">Wszystkie filmy</h3>

		<div class="btn-group" role="group" aria-label="Basic example">
			<button id="show-modal" class="btn btn-lg btn-success mb-4" @click="addMovie()">Dodaj film</button>
			<button type="button" class="btn btn-lg btn-secondary mb-4" v-on:click="fetchMovies()">
				Pobierz filmy z API
			</button>
		</div>

		<Teleport to="body">
			<modal :show="showModal" @close="handleModalClose">
				<template #header>
					<h3>{{ modalHeader }}</h3>
				</template>
			</modal>
		</Teleport>

		<div v-if="message" class="alert alert-success">{{ message }}</div>
		<div class="container">
			<table class="table table-dark">
				<thead>
					<tr>
						<th class="text-orange">Id</th>
						<th>Tytuł</th>
						<th>Reżyser</th>
						<th>Rok wydania</th>
						<th>Ocena</th>
						<th></th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="movie in movies" v-bind:key="movie.id">
						<td>{{ movie.id }}</td>
						<td>{{ movie.title }}</td>
						<td>{{ movie.director }}</td>
						<td>{{ movie.year }}</td>
						<td>{{ movie.rate }}</td>

						<td>
							<button class="btn btn-info mx-1" v-on:click="editMovie(movie)">Edytuj</button>
							<button class="btn btn-danger" v-on:click="deleteMovie(movie.id)">Usuń</button>
						</td>
					</tr>
				</tbody>
			</table>
			<Modal :selectedMovie="selectedMovie"></Modal>
		</div>
	</div>
</template>

<script>
import MovieDataService from '../service/MovieDataService'
import Modal from './AddMovieModal.vue'

export default {
	name: 'Movies',
	data() {
		return {
			movies: [],
			message: '',
			showModal: false,
			shouldRefresh: false,
			selectedMovie: null,
			modalHeader: null,
		}
	},
	components: {
		Modal,
	},
	methods: {
		refreshMovies() {
			MovieDataService.retrieveAllMovies().then(res => {
				var result = res.data
				this.movies = result.data
			})
		},
		addMovie() {
			this.modalHeader = 'Dodaj film'
			this.showModal = true
		},
		updateMovie(id) {
			this.$router.push(`/movie/${id}`)
		},
		deleteMovie(id) {
			MovieDataService.deleteMovie(id).then(() => {
				this.refreshMovies()
			})
		},
		editMovie(movie) {
			this.modalHeader = 'Edytuj film'
			this.selectedMovie = movie
			this.showModal = true
		},
		fetchMovies() {
			MovieDataService.fetchMovies().then(() => {
				this.refreshMovies()
			})
		},
		handleModalClose() {
			this.showModal = false
			this.selectedMovie = null
			this.shouldRefresh = true
		},
	},
	created() {
		this.refreshMovies()
	},
	watch: {
		shouldRefresh(newVal) {
			if (newVal) {
				this.refreshMovies(), (this.shouldRefresh = false)
			}
		},
	},
}
</script>

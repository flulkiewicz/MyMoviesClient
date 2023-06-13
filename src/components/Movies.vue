<template>
	<div class="container bg-dark p-5 rounded text-orange">
		<h3 class="mb-3">Wszystkie filmy</h3>

		<button id="show-modal" class="btn btn-lg btn-success mb-4" @click="showModal = true">Dodaj film</button>
		<Teleport to="body">
			<!-- use the modal component, pass in the prop -->
			<modal :show="showModal" @close="showModal = false">
				<template #header>
					<h3>Dodaj film</h3>
				</template>
			</modal>
		</Teleport>
    
		<div v-if="message" class="alert alert-success">{{ this.message }}</div>
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
							<button class="btn btn-danger" v-on:click="deleteMovie(movie.id)">Usuń</button>
						</td>
					</tr>
				</tbody>
			</table>
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
			this.$router.push(`/movie/-1`)
		},
		updateMovie(id) {
			this.$router.push(`/movie/${id}`)
		},
		deleteMovie(id) {
			MovieDataService.deleteMovie(id).then(() => {
				this.refreshMovies()
			})
		},
	},
	created() {
		this.refreshMovies()
	},
}
</script>

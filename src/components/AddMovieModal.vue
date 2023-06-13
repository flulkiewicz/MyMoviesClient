<template>
	<Transition name="modal">
		<div v-if="show" class="modal-mask">
			<div class="modal-container">
				<div class="modal-header">
					<slot name="header">Dodaj film</slot>
				</div>

				<div class="modal-body text-white">
					<form>
						<div class="form-group">
							<label for="title">Tytuł:</label>
							<input type="text" class="form-control m-2" id="title" v-model="movie.title" />
						</div>
						<div class="form-group">
							<label for="director">Reżyser:</label>
							<input type="text" class="form-control m-2" id="director" v-model="movie.director" />
						</div>
						<div class="form-row">
							<div class="form-group col-md-6">
								<label for="year">Rok wydania:</label>
								<input type="number" class="form-control m-2" id="year" v-model="movie.year" />
							</div>
							<div class="form-group col-md-6">
								<label for="rate">Ocena:</label>
								<input type="number" class="form-control m-2" id="rate" v-model="movie.rate" />
							</div>
						</div>
					</form>
				</div>
				<div class="modal-footer">
					<slot name="footer">
						<div class="btn-group" role="group" aria-label="Basic example">
							<button class="btn btn-success modal-default-button" @click="addMovie">Dodaj</button>
							<button class="btn btn-warning modal-default-button" @click="$emit('close'), clearForm()">Cofnij</button>
						</div>
					</slot>
				</div>
			</div>
		</div>
	</Transition>
</template>

<script>
import MovieDataService from '../service/MovieDataService'
import { useVuelidate } from '@vuelidate/core'
import { required } from '@vuelidate/validators'

export default {
	props: {
		show: Boolean,
	},
	data() {
		return {
			movie: {
				title: '',
				director: '',
				year: '',
				rate: '',
			},
			v$: useVuelidate(),
		}
	},
	validations() {
		return {
			title: {
				required,
			},
			director: {
				required,
			},
			year: {
				required,
			},
			rate: {
				required,
			},
		}
	},
	methods: {
		addMovie() {
			this.v$.$validate()
			if (!this.v$.$error) {
				MovieDataService.createMovie(this.movie).then(() => {
					this.clearForm()
					this.$emit('close')
				})
			} else {
				alert('Validation error')
			}
		},
		clearForm() {
			this.movie = {
				title: '',
				director: '',
				year: '',
				rate: '',
			}
		},
	},
}
</script>

<style></style>

  <template>
    <div class="container">
      <h3>Wszystkie filmy </h3>
      <div v-if="message" class="alert alert-success">{{ this.message }}</div>
      <div class="container">
        <table class="table">
          <thead>
            <tr>
              <th>Id</th>
              <th>Tytuł</th>
              <th>Reżyser</th>
              <th>Rok wydania</th>
              <th>Ocena</th>
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
               <button class="btn btn-danger" v-on:click="deleteMovie(movie.id)">
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="row">
          <button class="btn btn-success" v-on:click="addMovie()">Add</button>
        </div>
      </div>
    </div>
  </template>

<script>
import MovieDataService from "../service/MovieDataService";

export default {
  name: "Movies",
  data() {
    return {
      movies: [],
      message: "",
    };
  },
  methods: {
    refreshMovies() {
      MovieDataService.retrieveAllMovies().then((res) => {
        var result = res.data;
        this.movies = result.data
      });
    },
    addMovie() {
      this.$router.push(`/movie/-1`);
    },
    updateMovie(id) {
      this.$router.push(`/movie/${id}`);
    },
    deleteMovie(id) {
      MovieDataService.deleteMovie(id).then(() => {
        this.refreshMovies();
      });
    },
  },
  created() {
    this.refreshMovies();
  },
};
</script>


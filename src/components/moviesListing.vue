<template>
<div>
 <div class="container-fluid">
        <div class="row">
            <div class="col-md-6 col-12">
                <div id="movieList">
                    <div class="header">
                        <h1>{{ header}}</h1>
                        <p class="smallText">(click once to indicate a movie has been watched and double click to delete)</p>
                    </div>
                    <div class="movie-info">
                        
                        <div v-if="state === 'edit'" class="add-movie-form">
                            <input v-model="newMovie" type="text" placeholder="Add a movie title" @keyup.enter="saveMovie">
                            <button class="btn btn-primary" :disabled="newMovie.length === 0" @click="saveMovie">Save to Que</button>

                        </div>
                        <div class="buttonGroup">
                            <button v-if="state === 'default'" class="btn btn-primary mx-auto" @click="changeState('edit')">Add a new movie</button>
                            <button v-else class="btn btn-danger" @click="changeState('default')">Cancel</button>

                        </div>
                    </div>
                    <ul class="moviesList">
                        <li v-for="movie in reversedMovies" :class="{strikeout: movie.watched}" @click="toggleWatched(movie)" @dblclick="deleteMovie(movie)" :key="movie.title">{{ movie.title}}</li>
                    </ul>
                    <div class="messages">
                        <p v-if="movies.length < 5">Time to add more movies!</p>
                        <p>You currently have
                            <span class="red"> {{ movies.length }}</span> movies in your Que!</p>
                          <p v-if="!watched">You have watched {{watchedCount()}} movies.</p>


                        <button class="btn btn-lg btn-secondary" @click="deleteAllMovies(index)">Clear Movies</button>
                    </div>

                </div>
            </div>
        </div>
    </div>
    </div>
</template>

<script>
        export default {
            el: '#movieList',
            data: {
                state: 'default',
                header: 'Bruce\'s Movie List',
                newMovie: '',
                watched: '',
                movies: [{
                        title: 'Star Wars',
                        watched: false,
                    },
                    {
                        title: 'Jurrasic Park',
                        watched: true,

                    },
                    {
                        title: 'Frozen',
                        watched: false,

                    },
                ]
                
            },
            computed: {
                reversedMovies() {
                    return this.movies.slice(0).reverse();
                }
            },
            methods: {
                
                saveMovie: function () {
                    this.movies.push({
                        title: this.newMovie,
                        watched: false,
                        date: new Date().toLocaleString(),
                    });
 
                    this.newMovie = '';
                    this.watchedMovie = '';
                },
               
                deleteMovie: function (index) {
                    this.movies.splice(index, 1);
                },
                deleteAllMovies: function (index) {
                    this.movies.splice(index);
                },
                changeState: function (newState) {
                    this.state = newState;
                    this.newMovie = '';
                },
                toggleWatched: function (movie) {
                    movie.watched = !movie.watched;
                }
                

            },
            watch: {
                movies: {
                    handler() {
                        this.newMovie = '';
                        localStorage.setItem('movies', JSON.stringify(this.movies));
                    },
                    
                    deep: true,
                },
            },
            mounted() {
                console.log('App mounted!');
                if (localStorage.getItem('movies')) this.movies = JSON.parse(localStorage.getItem('movies'));
            },
        }
    </script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

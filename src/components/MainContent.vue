<template>
    <div class="back">
        <div class="container">
            <ul>
                <li class="text-center" v-for="item, index in filmList" :key="index">
                    <h3>{{item.title}}</h3>
                    <div>Titolo originale: {{item.original_title}}</div>
                    <div>Lingua: {{item.original_language}}</div>
                    <div>Voto: {{item.vote_average}}</div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'MainContent',
    props: ['text'],
    data() {
        return {
            url: 'https://api.themoviedb.org/3/search/movie?api_key=f2faa276f128d7e15803d0612da76a93&language=it-IT&query=a&page=1&include_adult=false',
            filmList: [],
            userText: '',
        }
    },
    methods: {
        getFilm() {
            axios.get(this.url).then((result) =>{
                this.filmList = result.data.results;
            })
        },
        searchFilm() {
            this.userText = this.text;
            this.url = `https://api.themoviedb.org/3/search/movie?api_key=f2faa276f128d7e15803d0612da76a93&language=it-IT&query=${this.text}&page=1&include_adult=false`;
            this.filmList = [];
            this.getFilm();
        }
    },
    watch: {
        text() {
            this.searchFilm();
        }
    },
    created() {
        this.getFilm();
    }
}
</script>

<style lang="scss" scoped>
@import '../assets/style.scss';
.back {
    background-color: grey;
}
ul {
    list-style-type: none;
    display: flex;
    justify-content: space-evenly;
    flex-wrap: wrap;
    li {
        background-color: black;
        color: white;
        margin: 15px;
        width: 250px;
    }
}
</style>
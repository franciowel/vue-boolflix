<template>
    <div class="back">

        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.1.1/css/all.min.css" integrity="sha512-KfkfwYDsLkIlwQp6LFnl8zNdLGxu9YAA1QvwINks4PhcElQSvqcyVLLD9aMhXd13uQjoXtEKNosOWaZqXgel0g==" crossorigin="anonymous" referrerpolicy="no-referrer" />

        <div class="container">

            <div class="d-flex justify-content-center">
                Lingua: 
                <select @change="selectLang" name="lang" id="lang">
                    <option v-for="item, index in langList" :key="index" :value="item.iso_639_1">{{item.english_name}}</option>
                </select>
            </div>

            <h2>Film</h2>
            <ul>
                <li class="text-center" v-for="item, index in filmList" :key="index">

                    <div class="cover">
                        <img :src="`https://image.tmdb.org/t/p/w342/${ item.poster_path }`" :alt="item.title">
                    </div>
                    <!-- ELEMENTI NASCONTI DA FAR VEDERE HOVER -->
                    <!-- INFORMAZIONI DEL FILM -->
                    <div class="hidden">
                        <h3>{{item.title}}</h3>
                        <div>Titolo originale: {{item.original_title}}</div>
                        <!-- LINGUA -->
                        <div class="language" v-if="item.original_language == 'en'">Lingua: {{item.original_language}}</div>
                        <div v-else>
                            <img
                            class="flag"
                            :src="`https://flagcdn.com/16x12/${item.original_language}.png`"
                            :srcset="`https://flagcdn.com/32x24/${item.original_language}.png 2x,
                                    https://flagcdn.com/48x36/${item.original_language}.png 3x`"
                            width="20"
                            :alt="`${item.original_language}`">
                        </div>
                        <!-- VALUTAZIONE -->
                        <!-- <div>Voto: {{item.vote_average}}</div> -->

                        <div class="stars">
                            <i v-for="n in 5" :key="n" class="fa-regular fa-star" :class="{'gold': n <= voteCalc(item.vote_average) }"></i>
                        </div>

                        <div class="overview">{{item.overview}}</div>

                    </div>
                </li>
            </ul>
            <!-- SERIE TV -->
            <h2>Serie TV</h2>

            <ul>
                <li class="text-center" v-for="item, index in tvList" :key="index">
                    <div class="cover">
                        <img :src="`https://image.tmdb.org/t/p/w342/${ item.poster_path }`" :alt="item.name">
                    </div>
                    <!-- ELEMENTI NASCONTI DA FAR VEDERE HOVER -->
                    <div class="hidden">
                        <div class="title">Titolo: {{item.name}}</div>
                        <div class="title">Titolo originale: {{item.original_name}}</div>
                        <div v-if="item.original_language == 'en'">Lingua: {{item.original_language}}</div>
                        <div v-else>
                            <img
                            class="flag"
                            :src="`https://flagcdn.com/16x12/${item.original_language}.png`"
                            :srcset="`https://flagcdn.com/32x24/${item.original_language}.png 2x,
                                https://flagcdn.com/48x36/${item.original_language}.png 3x`"
                            width="20"
                            :alt="`${item.original_language}`">
                        </div>
                        <div class="stars">
                            <i v-for="n in 5" :key="n" class="fa-regular fa-star" :class="{'gold': n <= voteCalc(item.vote_average) }"></i>
                        </div>
                        <div class="overview">{{item.overview}}</div>
                    </div>
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
            urlTv: 'https://api.themoviedb.org/3/search/tv?page=1&query=a&include_adult=false&api_key=f2faa276f128d7e15803d0612da76a93&language=it-IT',
            langUrl: 'https://api.themoviedb.org/3/configuration/languages?api_key=f2faa276f128d7e15803d0612da76a93',
            filmList: [],
            tvList: [],
            langList: [],
            userLang: 'it',
            userLangCapital: 'IT',
            userText: '',
        }
    },
    methods: {
        // FUNZIONI DI RICERCA FILM
        getFilm() {
            axios.get(this.url).then((result) =>{
                this.filmList = result.data.results;
            });
            axios.get(this.urlTv).then((result) =>{
                this.tvList = result.data.results;
            });
        },
        searchFilm() {
            this.userText = this.text;
            this.url = `https://api.themoviedb.org/3/search/movie?api_key=f2faa276f128d7e15803d0612da76a93&language=${this.userLang}-${this.userLangCapital}&query=${this.text}&page=1&include_adult=false`;
            this.urlTv = `https://api.themoviedb.org/3/search/tv?page=1&query=${this.userText}&include_adult=false&api_key=f2faa276f128d7e15803d0612da76a93&language=${this.userLang}-${this.userLangCapital}`;
            this.filmList = [];
            this.tvList = [];
            this.getFilm();
        },
        // FUNZIONI LINGUA 
        selectLang(value) {
            this.userLang = event.target.value;
            this.userLangCapital = this.userLang.toLocaleUpperCase();
            console.log(value)
            this.searchFilm();
        },
        getLang() {
            axios.get(this.langUrl).then((result) =>{
                this.langList = result.data;
            });
        },
        // SISTEMA CALCOLO
        voteCalc(mark) {
            const count = mark / 2;
            return Math.round(count);
        },
    },      
    watch: {
        text() {
            this.searchFilm();
        }
    },
    created() {
        this.getFilm();
        this.getLang();
    },
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
        color: white;
        margin: 15px;
        width: 250px;
    }
}
h2 {
    color: white;
    font-weight: 700;
    font-size: 35px;
    text-align: center;
}
li {
    width: 345px;
    height: 515px;
}
img {
    width: 342px;
    max-width: 342px;
    height: 513px;
}
.gold {
    color: yellow;
}
.stars {
    font-size: 20px;
    margin: 15px;
}
li:hover {
    .cover {
        display: none;
    }
    .hidden {
        display: block;
    }
}
.title {
    font-size: 20px;
    margin: 15px;
}
.title:first-of-type {
    margin-top: 40px;
}

.flag {
    height: 20px;
    width: 25px;
    margin: 15px;
}
.lang {
    font-size: 20px;
    margin: 15px;
}
.cover {
    display: block;
}
.overview {
    margin: 15px; 
    overflow-y: auto;
    max-height: 400px;
}
.hidden {
    display: none;
    max-height: 515px;
    max-width: 345px;
    overflow-y: auto;
}
</style>
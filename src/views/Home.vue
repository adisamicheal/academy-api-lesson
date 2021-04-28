<template>
  <div class="home">
    <h1 style="text-align: center; color: green; padding: 20px 0">How to Consume api with axios</h1>
    <div v-if="articles && articles.length != 0" class="article_container">
        <div v-for="(article, index) in articles" :key="index" class="cards">
          <a :href="article.url" target="_blank">
            <h1 class="title">{{ article.title }}</h1>
            <div class="card-image">
              <img :src="article.urlToImage" alt="" v-if="article.urlToImage !== null">
              <img src="../assets/logo.png" alt="" v-else>
            </div>
            <p>{{ article.content }}</p>
            <small>{{ article.author }}</small>
            <h5> {{ article.publishedAt | formattedDate }}</h5>
          </a>
        </div>
    </div>
    <div v-else>
       Loading...
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
import moment from 'moment'

export default {
  name: 'Home',
  data() {
    return {
      // info: null,
      news: null,
      articles: null,
      error: false,
    }
  },
  components: {
  },
  // lifecycle hooks
  mounted() {
    console.log(this.returnApi);
    this.getDataFromNewsApi()
  },
  filters:{
    formattedDate(value) {
      return moment(value).format('llll')
    }
  },
  computed: {
    filteredDate() {
      return this.articles.filter(article => article.publishedAt)
    }
  },
  methods: {
    getDataFromNewsApi() {
      axios.get(`https://newsapi.org/v2/everything?q=tesla&from=2021-03-27&sortBy=publishedAt&apiKey=58eec6a66ff546e19495bcb37f068f71`)
      .then((res) => {
        this.news = res
        this.articles = res.data.articles
      })
      .catch((err) => {
        console.log(err);
        this.error = true
      })
    },
  }
}
</script>

<style scoped>
.home {
  background: #ecfdf7;
}
.article_container{
  max-width: 700px;
  display: block;
  margin: auto;
  display: grid;
  grid-template-columns: 1fr 1fr;
}
.title {
  font-size: 24px;
  color: green;
}
.cards {
  background: #f9fbfc;
  padding: 20px;
  margin-top: 20px;
  margin-left: 10px;
  border-radius: 20px;
}
.card-image img {
  width: 100%;
  margin: 20px 0;
  /* height: 200px; */
}
p {
  font-size: 16px;
  line-height: 24px;
}
small {
  color: green;
}
</style>
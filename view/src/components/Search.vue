<template>
  <div class="content">
    <div class="wrapp">
      <div class="post" v-for="article in articles">
        <div class="top">
          <div class="title">
            <a class="p-title" rel="bookmark" v-link="'/article/' + article.id">
              <i class="fa fa-file-text-o"></i><span>{{ article.title }}</span>
            </a>
          </div>
          <div class="info">
            <div class="white">
              <span class="date">{{ new Date(article.createdAt).toLocaleString() }}</span>
              <span class="dot"> - </span>
              <span class="comment">{{ article.commentNum + ' 条评论' }}</span>
            </div>
          </div>
        </div>
        <div class="entry">{{{ article.content }}}</div>
        <div class="bottom">
          <div class="left">
            <a class="more ripple" v-link="'/article/' + article.id"><i
              class="fa fa-caret-square-o-right"></i>阅读全文</a>
          </div>
        </div>
      </div>
      <div class="content-loading" v-show="isLoading">
        <img src="../assets/loading.gif">
      </div>
      <div class="content-loading" v-show="isNotFound">
        <p>再怎么看也没有啦 ╮(╯▽╰)╭</p>
      </div>
      <span class="line-right line"></span>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        articles: [],
        isLoading: true
      }
    },
    computed: {
      isNotFound() {
        return (this.articles.length == 0) && (this.isLoading == false)
      }
    },
    methods: {
      showArticles(keyword) {
        this.$http.get('/api/search?keyword=' + keyword).then((response) => {
          this.isLoading = false
          this.articles = response.body
        }, (response) => {
          console.log(response)
        })
      },
      showLoading() {
        this.isLoading = true
        this.articles = []
      }
    },
    route: {
      data ({to: {params: {keyword}}}) {
        this.showLoading()
        this.showArticles(keyword)
        document.title = 'Volio\'s Blog'
      }
    }
  }
</script>

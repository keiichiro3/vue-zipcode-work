<template>
  <div id="app">
    <h1>ZIPCODE</h1>
    <form @submit.prevent="doSubmit">
      <input v-model="zipcode" placeholder="郵便番号">
      <button type="submit">送信</button>
    </form>
    <div>
      <h2 class="serch-result">検索結果</h2>
      <p class="result" v-for="(item, index) in address" :key="index">{{ item }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  methods: {
    doSubmit: function() {
      const jsonp = require('jsonp')
      jsonp('http://zipcloud.ibsnet.co.jp/api/search?zipcode=' + this.zipcode, null, (err, data) => {
        if (err) {
          console.log(err)
        } else {
          var tmp = []
          data.results.forEach(function(val, index, array) {
            this.push(val.address1 + val.address2 + val.address3)
          }, tmp)
          this.address = tmp
        }
      })
    }
  },
  data () {
    return {
      zipcode: '',
      address: []
    }
  }
}
</script>

<style>
body {
  background-size: cover; 
  background-image: url("./assets/rikka-wallpaper2.png")

}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

li {
  list-style: none;
}

.result {
  font-size: 2rem;
  margin: 0;
}

.serch-result {
  margin-bottom: 5px;
}
</style>
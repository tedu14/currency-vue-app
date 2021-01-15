<template>
  <div class="container grid-lg my-2 py-2 text-left">
    <div class="card mb-2" v-if="listenQuotes.length > 0">
      <div class="card-header">
        <div class="h4 text-center">
          Seguindo
        </div>
      </div>
      <div class="card-body">
        <watch-list-quotes @unlisten="onUnlisten" :listenQuotes="listenQuotes"></watch-list-quotes>
      </div>
    </div>
    <div class="card">
    <div class="card-header">
      <div class="h4 text-center">
        Todas as moedas
      </div>
    </div>
    <div class="card-body">
      <list-quotes 
        :quotes="quotes" 
        :listenQuotes="listenQuotes"
        @listen="onListen"
        @unlisten="onUnlisten"
      >
      </list-quotes>
    </div>
  </div>
  </div>
</template>

<script>
import { onMounted, reactive, toRefs } from 'vue'
import api from './services/api'
import ListQuotes from './components/ListQuotes.vue'
import WatchListQuotes from './components/WatchListQuotes.vue'

export default {
  name: 'App',
  components: {
    ListQuotes,
    WatchListQuotes
  },
  setup(){

    const data = reactive({
      quotes: {},
      listenQuotes: []
    })


    onMounted(async () => {
      const response = await api.all()

      data.quotes = response.data
    })

    function onListen(code){
      data.listenQuotes.push(code)
    }

    function onUnlisten(code){
      data.listenQuotes = data.listenQuotes.filter(key => key !== code)
    }


    return {
      ...toRefs(data),
      onListen,
      onUnlisten
    }
 
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

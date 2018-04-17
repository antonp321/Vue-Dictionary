<template>
  <div id="app">
    <dictionary-header></dictionary-header>
    <dictionary-log :words="words"></dictionary-log>
  </div>
</template>

<script>
import DictionaryHeader from './components/DictionaryHeader.vue';
import DictionaryLog from './components/DictionaryLog.vue';

export default {
  components: {
    'dictionary-header': DictionaryHeader,
    'dictionary-log' : DictionaryLog
  },
  data(){
    return {
      words: []
    }
  },
  created(){
    this.$http.get('https://api.myjson.com/bins/7pkyz').then(response => {
        this.words = response.body.words;
    })
  }
}
</script>

<style>

</style>

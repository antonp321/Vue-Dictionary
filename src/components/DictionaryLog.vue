<template>
  <div>
    <h4>Резултати <span class="text-muted">({{numberOfWords}})</span></h4>
    <ul>
      <li v-for="word in wordsToShow">{{word}}</li>
    </ul>
    <my-pagination :pageMax="pageMax" :wordsToMatch="wordsToMatch"></my-pagination>
  </div>
</template>

<script>
  import {eventBus} from '.././main.js'
  import Pagination from './Pagination.vue'

  export default {
    components: {
      'my-pagination': Pagination
    },
    props:['words'],
    data(){
        return {
            inputStr: "",
            reverseArray: false,
            longWords: false,
            pageNumber: 1,
            elementsPerPage: 0
        }
    },
    computed: {
      wordsToMatch(){
        let arrayToReturn = [];

        if(this.inputStr.length > 0) {
          arrayToReturn = this.words.filter((element) => {
            if(this.longWords) {
                if(element.length > 6){
                  return element.match(this.inputStr);
                }
            }
            else{
              return element.match(this.inputStr);
            }
          });

        }

        arrayToReturn.sort();

        if(this.reverseArray){
          arrayToReturn = arrayToReturn.reverse();
        }

        return arrayToReturn;
      },
      numberOfWords(){
          return this.wordsToShow.length;
      },
      pageMax(){
        return Math.ceil(this.wordsToMatch.length / 20);
      },
      wordsToShow(){
          let startNumberToSlice = 0;
          let endNumberToSlice = 0;

          if(this.pageNumber === 1){
              startNumberToSlice = 0;
              endNumberToSlice = this.elementsPerPage;
          }
          else {
              startNumberToSlice = (this.pageNumber - 1) * this.elementsPerPage;

              if(this.wordsToMatch.length > startNumberToSlice + this.elementsPerPage){
                  endNumberToSlice = startNumberToSlice + this.elementsPerPage;
              }
              else{
                  endNumberToSlice = this.wordsToMatch.length;
              }
          }

          let wordsToMatchNewInstance = this.wordsToMatch;

        return wordsToMatchNewInstance.slice(startNumberToSlice, endNumberToSlice);
      }
    },
    created(){
        eventBus.$on('inputStringChanged',(data) => {
            this.inputStr = data;
        });

        eventBus.$on('reverseArrayClicked',(data) => {
          this.reverseArray = data;
        });

        eventBus.$on('longWordsClicked',(data) => {
          this.longWords = data;
        });

        eventBus.$on('currentPagesWasChanged',(data) => {
          this.pageNumber = data.currentPage;
          this.elementsPerPage = data.elementsPerPage;
        });
    }
  }
</script>

<style scoped>
  h4 {
    margin-top:18px;
    margin-left: 2%;
  }
</style>

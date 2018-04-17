<template>
  <nav class="pages" v-if="showPagination">
    <ul class="pagination">
      <li class="page-item previous" @click="changeCurrentPage(false)"><a class="page-link"><span>«</span></a></li>
      <li><span class="page-link"><span>{{currentPage}}</span></span></li>
      <li class="page-item next"@click="changeCurrentPage(true)"><a class="page-link"><span>»</span></a></li>
    </ul>
  </nav>
</template>

<script>
  import {eventBus} from '.././main.js'

  export default {
    props: ['pageMax','wordsToMatch'],
    data(){
        return {
            currentPage: 1,
            numberOfElementsPerPage: 20
        }
    },
    computed: {
      showPagination(){

        eventBus.$emit('currentPagesWasChanged', {
          elementsPerPage: this.numberOfElementsPerPage,
          currentPage: this.currentPage
        });

        if(this.wordsToMatch.length > 20){
            return true;
          }
          else{
            return false;
          }
      }
    },
    methods: {
        changeCurrentPage(isNext){
          if(isNext && this.currentPage < this.pageMax){
              this.currentPage++;
              eventBus.$emit('currentPagesWasChanged', {
                  elementsPerPage: this.numberOfElementsPerPage,
                  currentPage: this.currentPage
              })
          }
          else if(!isNext && this.currentPage > 1){
            this.currentPage--;
            eventBus.$emit('currentPagesWasChanged', {
              elementsPerPage: this.numberOfElementsPerPage,
              currentPage: this.currentPage
            })
          }
        }
   },
    watch: {
        wordsToMatch(){
            console.log("ASd");
            this.currentPage = 1;
        }
    }
  }
</script>

<style scoped>
  .pages {
    padding-top:15px;
    border-top:1px solid #ddd;
    text-align:center;
  }
</style>

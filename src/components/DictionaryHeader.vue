<template>
  <div>
  <h1>Търсене на думи</h1>
    <form class="form-inline">
      <input class="form-control" type="text" placeholder="Въведете символи" id="search-field" v-model="inputString">
      <div class="form-check ml-md-3">
        <label class="form-check-label">
          <input class="form-check-input" type="checkbox" v-model="searchNow"> Търси Докато Въвеждаш</label>
      </div>
      <div class="form-check ml-md-3">
        <label class="form-check-label">
          <input class="form-check-input" type="checkbox" v-model="reversedOrder"> Обратен ред</label>
      </div>
      <div class="form-check ml-sm-3">
        <label class="form-check-label">
          <input class="form-check-input" type="checkbox" v-model="longWordsOnly"> Само дълги думи
        </label>
      </div>
      <button class="btn btn-primary ml-md-auto" type="button" id="search-button" @click.prevent="search">Търсене</button>
    </form>
  </div>
</template>

<script>
  import {eventBus} from '.././main.js'

  export default {
    data(){
      return {
        inputString: '',
        searchNow: false,
        longWordsOnly: false,
        reversedOrder: false
      }
    },
    methods: {
      search(){
        eventBus.$emit('inputStringChanged', this.inputString);
      }
    },
    watch: {
      inputString(value){
        if (this.searchNow) {
          eventBus.$emit('inputStringChanged', value);
        }
      },
      searchNow(){
        this.inputString = '';
      },
      reversedOrder(value){
        eventBus.$emit('reverseArrayClicked', value);
      },
      longWordsOnly(value){
        eventBus.$emit('longWordsClicked', value);
      }
    }
  }
</script>

<style scoped>
  h1 {
    margin-bottom:30px;
    border-bottom:1px solid #ddd;
    padding-bottom:20px;
  }

  form {
    padding-bottom:35px;
    border-bottom:1px solid #ddd;
  }

  @media (min-width: 992px) {
    #search-field {
      min-width:300px;
    }
    #search-button {
      min-width:240px;
    }
  }

  @media (max-width: 767px) {
    #search-field {
      width:100%;
      margin-bottom:14px;
    }
    #search-button {
      width:100%;
      margin-top:20px;
    }
  }
</style>

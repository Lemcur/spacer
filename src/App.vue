<template>
  <div :class="[{ flexStart: step === 1 }, 'wrapper'] " >
    <transition name="slide">
      <img src="./assets/logo.svg" class="logo" alt="logo Spacer" v-if="step===1" />
    </transition>
    <transition name="fade">
      <HeroImage v-if="step === 0" />
    </transition>
    <Claim v-if="step === 0" />
    <SearchInput 
    v-model="searchValue" 
    @input="handleInput"
    :dark="step === 1"
    />
    <div class="results" v-if="results && !loading && step===1">
      <Item 
        v-for="item in results" 
        :item="item" :key="item.data[0].nasa_id" 
        @click.native="handleModalOpen(item)"  
      />
    </div>
    <Modal 
    v-if="modalOpen"
    @closeModal="modalOpen = false"
    :item="modalItem"
    />
  </div>
</template>

<script>
  // @ is an alias to /src
  import axios from "axios";
  import { debounce } from "lodash"
  import Claim from '@/components/Claim.vue'
  import SearchInput from '@/components/SearchInput.vue'
  import HeroImage from '@/components/HeroImage.vue'
  import Item from '@/components/Item.vue'
  import Modal from '@/components/Modal.vue'

  const API = 'https://images-api.nasa.gov/search';
  export default {
    name: 'Search',
      components: {
        Claim,
        SearchInput,
        HeroImage,
        Item,
        Modal,
      },
    data() {
      return {
        modalItem: null,
        modalOpen: false,
        loading: false,
        step: 0,
        searchValue: '',
        results: [],
      };
    },
    methods: {
      handleInput: debounce(function() {
        this.loading = true;
        axios.get(`${API}?q=${this.searchValue}&media_type=image`)
          .then((response) =>{
            this.results = response.data.collection.items;
            this.loading = false;
            this.step=1;
          })
          .catch((error) => console.log(error))
      }, 1000),
      handleModalOpen(item){
        this.modalOpen = true;
        this.modalItem = item;
      }
    }
  };
  </script>

<style lang="scss">
  .wrapper {
    margin: 0;
    position: relative;
    padding: 30px;
    width:100%;
    height:100vh; 
    display:flex;
    flex-direction:column;
    align-items: center;
    justify-content: center;
    &.flexStart {
      justify-content: flex-start;
    }  
  }

  @import url('https://fonts.googleapis.com/css?family=Montserrat:400,600,800');
  
  * {
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  body {
    font-family: 'Montserrat', sans-serif;
    margin:0;
    padding:0;
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .4s ease;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
  .logo{
    position: absolute;
    top:40px;
  }

  .slide-enter-active, .slide-leave-active {
    transition: margin-top .3 ease;
  }
  .slide-enter, .slide-leave-to {
    margin-top: -50px;
  }
  .results {
    margin-top: 40px;
    display: grid;
    grid-template-columns: 1fr;
    grid-gap:40px;
    @media (min-width: 768px) {
      width: 90%;
      grid-template-columns: 1fr 1fr 1fr;
    }
  }
</style>

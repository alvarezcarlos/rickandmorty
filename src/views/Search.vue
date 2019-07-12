<template>
  <div class="search">
    <label for="name"> Nombre:
      <input type="text" name="name" v-model="character" placeholder="Nombre del personaje">
    </label>
    
    <b-media 
        v-if="results.length"
        v-for="(result, index) in results" :key="index"
        tag="li" class="mb-5">
          <b-img 
          slot="aside" blank-color="#abc" 
          :src= "result.image" 
          width="64" alt="placeholder"></b-img>

          <h5 class="text-left pl-0 mt-0 mb-1">{{result.id}} {{result.name}}</h5>
          <p class="text-left ml-3 mb-0">
            <b>Origen:</b> {{result.origin.name}} <br>
            <b>Especie:</b> {{result.species}}<br>
            <b>Estatus:</b> {{result.status}}<br>
            <b>Género:</b> {{result.gender}}<br>
          </p>
        </b-media>
    
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data(){
    return{
      character:'',
      results:[]
    }
  },
  watch:{
    character: function(newValue, oldValue){
      axios
        .get(`https://rickandmortyapi.com/api/character/?name=${this.character}`)
        .then(response => {
          let info = response.data.info
          let data = response.data.results
          console.log(data[0].id)
          if(data[0].id != NaN){
            this.results = data
          }
        })
    
    }
  }
}
</script>

<template>
  <div class="container">
    <div class="container col-8 mb-5">
      <h6>Filtra los personajes</h6>
      <b-form-select v-model="selectedStatus" :options="status" size="sm" class="mt-3"></b-form-select>
      <b-form-select v-model="selectedGender" :options="gender" size="sm" class="mt-3"></b-form-select>
    </div>
    <ul class="list-unstyled">
      <b-media
        v-for="(character, index) in characters" :key="index"
        tag="li" class="mb-5">
        <b-img 
          slot="aside" blank-color="#abc" 
          :src= "character.image" 
          width="64" alt="placeholder"></b-img>
          <h5 class="text-left pl-0 mt-0 mb-1">{{character.id}} {{character.name}}</h5>
          <p class="text-left ml-3 mb-0">
            <b>Origen:</b> {{character.origin.name}} <br>
            <b>Especie:</b> {{character.species}}<br>
            <b>Estatus:</b> {{character.status}}<br>
            <b>Género:</b> {{character.gender}}<br>
          </p>
      </b-media>
    </ul>
  </div>
 </template>

<script>
import axios from 'axios'

export default {
  data(){
    return{
      characters: [],
      api: 'https://rickandmortyapi.com/api/character/?page=',
      selectedStatus: '',
      status: [
          { value: '', text: 'Seleccione un estado' },
          { value: 'alive', text: 'vivo' },
          { value: 'dead', text: 'muerto' },
          { value: 'unknown', text: 'desconocido' }
        ],
      selectedGender: '',
      gender: [
          { value: '', text: 'Seleccione un género' },
          { value: 'female', text: 'femenino' },
          { value: 'male', text: 'masculino' },
          { value: 'genderless', text: 'sin genero' },
          { value: 'unknown', text: 'desconocido' }
        ],

    }
  },
  mounted: function(){
    axios
      .get(this.api)
      .then(response => {
        let arr = response.data.results
        let counter = 2
        for(let i = 0; i < response.data.info.pages - 2; i++){
          axios
            .get(this.api + counter)
            .then(response => {
              response.data.results.forEach(element => {
                arr.push(element)
              });
            })
          counter ++
        }
      this.characters = arr
    })
  },
  watch: {
    selectedStatus: function(){
      axios
      .get(`https://rickandmortyapi.com/api/character/?status=${this.selectedStatus}&gender=${this.selectedGender}`)
      .then(response => {
        this.characters = response.data.results
        let info = response.data.info
          if(info.pages > 0){
            for(let i = 2; i < info.pages - 1; i++){
              axios
               .get(`https://rickandmortyapi.com/api/character/?page=${i}&status=${this.selectedStatus}&gender=${this.selectedGender}`)
               .then(response => {
                  response.data.results.forEach(element => {
                  this.characters.push(element)
                  })
               })
            }
          }
      })
    },
    selectedGender: function(){
      axios
      .get(`https://rickandmortyapi.com/api/character/?status=${this.selectedStatus}&gender=${this.selectedGender}`)
      .then(response => {
        this.characters = response.data.results
        let info = response.data.info
          if(info.pages > 0){
            for(let i = 2; i < info.pages - 1; i++){
              axios
               .get(`https://rickandmortyapi.com/api/character/?page=${i}&status=${this.selectedStatus}&gender=${this.selectedGender}`)
               .then(response => {
                  response.data.results.forEach(element => {
                  this.characters.push(element)
                  })
               })
            }
          }
      })
    }
  },
}


</script>


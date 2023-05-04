<template>
    <div class="contenedor">
    <h2>Simple Fetch Usuarios</h2><p>people Users</p>
    <table  class="table border">
          <thead  >
            <tr>
              <th scope="col">#</th>
              <th scope="col">Nombre</th>
     
   
            </tr>
          </thead>
          <tbody>
          
            <tr v-for="usuario in users" :key="usuario.id">
              <th scope="row"> </th>
              <td>{{ usuario }}</td>
             
    
  
            </tr>
          </tbody>
        </table></div>
  </template>
  
  <script>
  import { ref, onMounted } from "vue"
  export default {
    name: "SimpleFetch",
    setup () {
      const users = ref(null)
      onMounted(async () => {
        const response = await fetch("https://api.themoviedb.org/3/tv/popular?api_key=40fddecbab890365ae15a4c7872d2c00");
        users.value = await response.json()
      })
  
      return { users }
    }
  }
  </script>
  
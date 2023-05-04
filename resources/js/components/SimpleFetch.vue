<template>
  <div class="contenedor">
  <h2>Simple Fetch Usuarios</h2><p>people Users</p>
  <table  class="table border">
        <thead  >
          <tr>
            <th scope="col">#</th>
            <th scope="col">Nombre</th>
            <th scope="col">email</th>
            <th scope="col">city</th>
            <th scope="col">city.street</th>
            <th scope="col">city.lat</th>
            <th scope="col">city.lng</th>
 
          </tr>
        </thead>
        <tbody>
          <tr v-for="usuario in users" :key="usuario.id">
            <th scope="row">{{ usuario.id }}</th>
            <td>{{ usuario.name }}</td>
            <td>{{ usuario.email }}</td>
            <td>{{ usuario.address.city }}</td>
            <td>{{ usuario.address.street}}</td>

            <td>{{ usuario.address.geo.lat }}</td>
            <td>{{ usuario.address.geo.lng }}</td>

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
      const response = await fetch("https://jsonplaceholder.typicode.com/users");
      users.value = await response.json()
    })

    return { users }
  }
}
</script>

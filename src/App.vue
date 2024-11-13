<template>
  <div class="container-fluid">
    <form @submit.prevent="createUser">
      <input type="text" class="form-control" v-model="newUser.nombre" placeholder="Nombre">
      <input type="text" class="form-control" v-model="newUser.apellido" placeholder="Apellido">
      <input type="number" class="form-control" v-model="newUser.edad" placeholder="Edad">
      <input type="number" class="form-control" v-model="newUser.id" placeholder="Id">
      <button type="submit" class="btn btn-primary">Crear Usuario</button>
    </form>

    <ul>
      <li v-for="user in users" :key="user.id">
        {{ user.nombre }} {{ user.apellido }} ({{ user.edad }})
        <button @click="deleteUser(user.id)">Eliminar</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newUser: {
        nombre: '',
        apellido: '',
        edad: '',
        id: '',
      },
      users: []
    }
  },
  methods: {
    async createUser() {
      // Llamada a la API para crear un usuario
      const response = await fetch('https://manejodeusuarios-egfcaga6b4dgdue4.canadacentral-01.azurewebsites.net/api/usuario', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.newUser)
      })

      if (response.ok) {
        const newUser = await response.json()
        this.users.push(newUser)
        this.newUser = { nombre: '', apellido: '', edad: '' }
      } else {
        console.error('Error al crear el usuario')
      }
    },
    async deleteUser(userId) {
      // Llamada a la API para eliminar un usuario
      await fetch(`https://manejodeusuarios-egfcaga6b4dgdue4.canadacentral-01.azurewebsites.net/api/usuario/${userId}`, {
        method: 'DELETE'
      })

      this.users = this.users.filter(user => user.id !== userId)
    },
    async fetchUsers() {
      const response = await fetch('https://manejodeusuarios-egfcaga6b4dgdue4.canadacentral-01.azurewebsites.net/api/usuario');
      const data = await response.json();
      this.users = data;
    }
  },mounted() {
    this.fetchUsers();
  }
}
</script>

<style scoped>
@import 'https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css';
/* Estilos generales */
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

/* Estilos para los encabezados */
h1 {
  color: #333;
  text-align: center;
}

/* Estilos para los párrafos */
p {
  font-size: 16px;
  line-height: 1.5;
  color: #666;
}

/* Estilos para los botones */
button {
  padding: 2px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
}
</style>
<template>
  <div class="container">
    <table class="table table-striped">
      <thead>
      <tr>
        <th>
          id
        </th>
        <th>
          Full Name
        </th>
      </tr>
      </thead>
      <tbody>
        <tr v-for="client in clients" :key="client.id">
          <td>{{client.id}}</td>
          <td>{{client.fullName}}</td>
          <td><button type="submit" class="btn" @click="deleteClient(client.id)"><vue-fontawesome icon="trash" size="1.5" color="red"></vue-fontawesome></button></td>
        </tr>
      </tbody>
    </table>
    <form>
      <p>Add a  client</p>
      <div class="form-group">
        <label for="inputId" class="label">Id</label>
        <input type="text" name="id" id="inputId" placeholder="Enter id" class="form-control" v-model="id" @change="onlyNumber" @input="onlyNumber">
      </div>
      <div class="form-group">
        <label for="inputName" class="label">First Name and Last Name</label>
        <input type="text" name="fullName" id="inputName" placeholder="Ivan Popov" class="form-control" v-model="fullName">
      </div>
      <button type="submit" class="btn btn-success" @click="addClient">Add</button>
    </form>

  </div>
</template>

<script>
// @ is an alias to /src

import axios from 'axios'

export default {
  name: 'Home',
  data() {
    return {
      clients: [],
      id: null,
      fullName: ''
    }
  },
  methods: {
    clientList() {
      return  axios.get('http://localhost:3000/clients')
      .then((clients) => this.clients = clients.data)
    },

    addClient() {
      axios.post('http://localhost:3000/clients', {
        "id": +this.id,
        "fullName": this.fullName
      })
    },

   async deleteClient(id) {
     await axios.delete('http://localhost:3000/clients/' +id)
     await this.clientList()
    },

    onlyNumber() {
      this.id = this.id.replace(/[^0-9.]/g, "")
    }
  },
  mounted() {
    this.clientList()
  }
}
</script>

<style lang="scss">
  .label{
    float: left;
  }
</style>

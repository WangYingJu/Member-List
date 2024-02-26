<script setup>
import axios from 'axios'
</script>

<style scoped>
*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  padding: 0;
  margin: 0;
}

header {
  display: flex;
  align-items: center;
  margin: 1rem;
}

h1 {
  margin-right: 1rem;
}

table {
  border-collapse: collapse;
  table-layout: fixed;
  width: 100%;
}

thead {
  color: #000;
  font-size: 14px;
  text-align: left;
  padding: 8px 16px;
}

table tr {
  background: unset;
  border-bottom: 1px solid #eaeaea;
}

tbody tr:hover {
  background: #f8f8f8;
}

table th,
table td {
  padding: 8px;
}

table td {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  color: #6c757d;
  font-size: 14px;
}

button {
  border-radius: 50px;
  border: unset;
  background-color: #dc3545;
  color: #fff;
  padding: 8px 16px;
  cursor: pointer;
}

button:hover {
  background-color: #bf2837;
}

select {
  width: 100%;
  padding: 0;
  margin: 0;
  border: unset;
}

.v-enter-active,
.v-leave-active {
  transition: all 0.5s ease-in-out
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.background {
  width: 72px;
  height: 72px;
  border-radius: 100px;
  background-image: '{{ item.picture.medium }}';
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
}
</style>

<template>
  <header>
    <h1>Member List</h1>
    <button type="button" @click="refresh">REFRESH <i class="pi pi-refresh"></i></button>
  </header>
  <table>
    <thead>
      <tr>
        <th scope="col">Image</th>
        <th scope="col">Name</th>
        <th scope="col">Gender</th>
        <th scope="col">
          <select v-model="ageSelected">
            <option disabled value="">Age</option>
            <option value="up">Age up</option>
            <option value="down">Age down</option>
          </select>
        </th>
        <th scope="col">Email</th>
        <th scope="col">Delet</th>
      </tr>
    </thead>
    <!-- <tbody> -->
    <transition-group tag="tbody">
      <tr v-for="(item, index) in showData" :key="item">
        <td scope="row">
          <div class="background" :style="{ 'background-image': 'url(' + item.picture.medium + ')' }"></div>
        </td>
        <td>{{ item.name.title }} {{ item.name.first }} {{ item.name.last }}</td>
        <td>{{ item.gender }}</td>
        <td>{{ item.dob.age }}</td>
        <td>{{ item.email }}</td>
        <td><button type="button" @click="deletItem(index)">DELET <i class="pi pi-trash"></i></button></td>
      </tr>
    </transition-group>
    <!-- </tbody> -->
  </table>
</template>

<script>
export default {
  data() {
    return {
      data: [],
      text: '<img :src="item.picture.large" alt="">',
      ageSelected: '',
    }
  },
  methods: {
    deletItem(index) {
      this.data.splice(index, 1)
    },
    refresh() {
      this.data = [];
      axios.get('https://randomuser.me/api/?results=5')
        .then(response => {
          this.data = response.data.results;
        });
      this.ageSelected = ''
    }
  },
  mounted() {
    axios.get('https://randomuser.me/api/?results=5')
      .then(response => {
        this.data = response.data.results;
      })
  },
  computed: {
    showData() {
      if (this.ageSelected === 'up') {
        return this.data.sort((a, b) => a.dob.age - b.dob.age)
      } else if (this.ageSelected === 'down') {
        return this.data.sort((a, b) => b.dob.age - a.dob.age)
      } else {
        return this.data;
      }
    }
  }
}
</script>



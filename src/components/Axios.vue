<script setup>
import axios from 'axios';

defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>
<!-- Displaying the data in the template area -->
<template>   
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <h3>
      <!-- You’ve successfully created a project with
      <a href="https://vitejs.dev/" target="_blank" rel="noopener">Vite</a> +
      <a href="https://vuejs.org/" target="_blank" rel="noopener">Vue 3</a>. -->
    </h3>

    <div>
    <h1>Employee Data</h1>
    {{ apiData.data }} 
    <table class="employee-table">
      <thead class="employee-table-head">
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Email</th>
          <th>Profession</th>
          <th>Location</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="person in apiData" :key="person.id">
          <td>{{ person.id }}</td>
          <td>{{ person.name }}</td>
          <td>{{ person.email }}</td>
          <td>{{ person.profession }}</td>
          <td>{{ person.location }}</td>
        </tr>
      </tbody>
    </table>
    <div v-if="errorMessage">{{ errorMessage }}</div>
  </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      apiData: [],
      errorMessage: ''
    };
  },
  mounted() {
    this.fetchData();  // fetch the data when the component is mounted
  },
  methods: {
    fetchData() {
      axios.get('https://eck3.de/wp-json/randomTestApi/v1/data')   // peforming a get request to the api 
        .then(response => {
          this.apiData = response.data.data;        // Storing the received data in the component’s data property.
          console.log('Response:', response.data);
        })
        .catch(error => {
          this.errorMessage = 'Failed to fetch data from the API. Please try again later.';   // error handling
          console.error('Axios request error:', error);
        });
    }
  }
};
</script>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}


.employee-table {
  width: 100%;
  border-collapse: collapse;
  margin: 0 auto; 
}

.employee-table th,
.employee-table td {
  border: 1px solid #dddddd;
  padding: 8px;
}

.employee-table th {
  background-color: #f2f2f2;
}

.error-message {
  color: red;
  margin-top: 10px;
  text-align: center; 
}

.employee-table-head {
  color: black;
}
</style>

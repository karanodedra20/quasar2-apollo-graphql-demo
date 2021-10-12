<template>
  <h3>GraphQL Demo</h3>
  <h5>User Data</h5>

  <div v-if="loading">Loading...</div>
  <div v-else-if="error">Error: {{ error.message }}</div>

  <div v-else-if="result && result.people">
    <table id="customers">
      <tr>
        <th>Name</th>
        <th>About</th>
        <th>Country</th>
      </tr>
      <tr v-for="ppl of result.people" :key="ppl.name">
        <td>{{ ppl.name }}</td>
        <td>{{ ppl.aboutShort }}</td>
        <td>{{ ppl.location.country.name }}</td>
      </tr>
    </table>
  </div>
  <div v-else>Loading....</div>
</template>

<script>
import { useQuery } from "@vue/apollo-composable";
import gql from "graphql-tag";

export default {
  setup() {
    const { result, loading, error } = useQuery(gql`
      query getPeople {
        people {
          name
          aboutShort
          location {
            country {
              name
            }
          }
        }
      }
    `);

    return { result, loading, error };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#customers {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}
#customers td,
#customers th {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: center;
}
#customers tr:nth-child(even) {
  background-color: #f2f2f2;
}
#customers tr:hover {
  background-color: #ddd;
}
#customers th {
  padding-top: 12px;
  padding-bottom: 12px;
  background-color: #04aa6d;
  color: white;
}
</style>

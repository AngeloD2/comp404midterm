<template>
  <v-table>
    <thead>
      <tr>
        <th class="text-left">

        </th>
        <th class="text-left">
          Group Name
        </th>
        <th class="text-left">
          Email
        </th>
        <th class="text-left">
          Group Type
        </th>
        <th class="text-left">
          Description
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="group in environmentalgroups" :key="group.id">
        <td>{{ group.id }}</td>
        <td>{{ group.groupname }}</td>
        <td>{{ group.email }}</td>
        <td>{{ group.grouptype }}</td>
        <td style="word-wrap: break-word;">{{ group.description }}</td>
        <td>
        </td>
      </tr>
    </tbody>
  </v-table>
</template>

<script>
import axios from 'axios'


export default {
  data() {
    return {
      environmentalgroups: [],
    }
  },
  async created() {
    try {
      const res = await axios.get('http://localhost:3000/environmentalgroups')
      this.environmentalgroups = res.data
    } catch (error) {
      console.log(error)
    }
  }
}
</script>
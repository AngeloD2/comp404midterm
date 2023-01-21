<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
  <v-card class="mx-auto" max-width="300">
    <v-list density="comfortable">
      <v-list-subheader>Navigation</v-list-subheader>

      <RouterLink to="/"> <v-list-item active-color="secondary" text="Home" value="/">
          Home
          <v-list-item-title></v-list-item-title>
        </v-list-item>
      </RouterLink>


      <RouterLink to="register"> <v-list-item active-color="secondary" value="/register" text="Home">

          <v-list-item-title>Register your group</v-list-item-title>
        </v-list-item>
      </RouterLink>


      <v-list-subheader> Environmental Groups </v-list-subheader>

      <RouterLink to="groups"> <v-list-item active-color="secondary" value="/groups" text="Home">

          <v-list-item-title>Registered Groups</v-list-item-title>
        </v-list-item>
      </RouterLink>

      
      <RouterLink to="activities"> <v-list-item active-color="secondary" value="/activities" text="Home">

<v-list-item-title>Group Activities</v-list-item-title>
</v-list-item>
</RouterLink>


    </v-list>
  </v-card>
</template>

<script>

</script>

<style scoped>
a {
  text-decoration: none;
}
</style>
<script setup>
import { RouterView } from 'vue-router'
import LinkList from './components/LinkList.vue'
</script>

<template>
  <v-app theme="dark">
    
    <v-app-bar color="primary">
      <p class="text-h5 text-black" style="text-align:center; margin:0 auto;">
      Environmental Group Registration
    </p>
    </v-app-bar>

    <v-navigation-drawer color="light">
      <v-card>
        <LinkList/>
      </v-card>
    </v-navigation-drawer>

<v-main>
    
  <RouterView />

</v-main>

</v-app>
</template>

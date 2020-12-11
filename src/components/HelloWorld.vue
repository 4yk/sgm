<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
         Это <h1> {{world_name}} </h1> 


         
      </v-col>
    </v-row>
  </v-container>
</template>

<script>

  export default {
    name: 'HelloWorld',

    data: () => ({
      world_name : "СПРАВОЧНИК",
    }),
    
    
  }
</script>

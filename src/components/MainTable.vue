<template>
  <div>
    <AppendForm 
    v-on:pupdate="updateDesserts"/>

    <v-data-table
      :hide-default-footer="false"
      :headers="headers"
      :items="filtrated_des"
      item-key="name"
      class="elevation-1"
      
    >
      
       
    <template v-for="h in headers"  v-slot:[`header.${h.value}`]  >

      <v-container  v-bind:key="h"  >
          <v-hover>
      <template v-slot:default="{ hover }">
        <v-card
          :elevation="hover ? 24 : 6"
          class="mx-auto "
        >
         
        <v-row no-gutters>
          <v-col
          cols="12"
          sm="11" class="text-center">
          <p class="display-1 mt-3">{{h.text}}</p>
          </v-col>
          <v-col
          cols="12"
          sm="1" >
           <ModalList 
           v-on:nf="itemHell"
           v-bind:filter_v = "h.value"
           v-bind:values="desserts"  />
 
          </v-col>
        </v-row>

        </v-card>
      </template>
    </v-hover>
      </v-container>
      
    </template>
    
    </v-data-table>
  </div>
</template>
<script>

  import ModalList from './ModalList';
  import AppendForm from './AppendForm';
  import axios from 'axios';
  export default {
    components:{
      ModalList,
      AppendForm,
    },
    data () {
      return {
   
        filter_dict:{},
        selected:[],
        filtrated_des:[],
        desserts: [],
      }
    },
    computed: {


      headers () {
        return [

          {
            text: 'ФИО',
            align: 'start',
            value: 'name',
          },
          {
            text: 'Телефон',
            value: 'phone',
            
          },
          { text: 'Отдел', value: 'dep'},
          { text: 'Организация', value: 'firm'},

        ]
      },
    },
    mounted: function(){


      axios
      .get('/phones')
      .then(response => (this.desserts = response.data, this.filtrated_des = this.search_item()));
   
     
        
    },
    methods: {

      search_item(){
        return this.desserts.filter(d => {
          return Object.keys(this.filter_dict).every(f => {
            return this.filter_dict[f].length < 1 || this.filter_dict[f].includes(d[f])
          })
        })
      },

      itemHell(values){
          this.filter_dict[values.pop()] = values;
          this.filtrated_des = this.search_item();
      },

      updateDesserts(values){
          if(values === 200){
            axios
            .get('/phones')
            .then(response => (this.desserts = response.data, this.filtrated_des = this.search_item()));
          }
           
      },

    
    },

   
}
</script>
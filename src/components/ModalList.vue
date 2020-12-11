<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      scrollable
      max-width="300px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="primary"
          dark
          v-bind="attrs"
          v-on="on"
          v-on:click="filtrate"
          class="mdi-filter-menu"
        >
           <v-icon>mdi-filter-menu</v-icon>
        </v-btn>
      </template>
      <v-card>
        <v-card-title>
          <v-text-field label="Фильтр" v-model="filter_text"></v-text-field>
        </v-card-title>
        <v-divider></v-divider>
        <v-card-text style="height: 300px;">
         <v-list-item v-for="item in filter_values" :key="item">
            <v-list-item-content>
            
                <v-checkbox
                  v-model="selected"
                  v-bind:label="item"
                  v-bind:value="item"
                ></v-checkbox>
                
              
            </v-list-item-content>
          </v-list-item>
        </v-card-text>
        <v-divider></v-divider>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
  export default {
    name: 'ModalList',
    props: ['values',
           'filter_v'],
    data: () => ({
      dialog: false,
      filter_text:"",
      filter_values:[],
      selected: [],
    
    }),
    methods:{
      filtrate(){
       
        this.filter_values = [];
        for(let item of this.values){
          this.filter_values.push(item[this.filter_v]);
        }
         
        
      },
    },
    watch: {
      dialog: function (){
          let ready_arr = this.selected.slice();

          
          if(this.dialog === false ){
            ready_arr.push(this.filter_v);
            this.$emit("nf", ready_arr); 
          }
        
      },
      filter_text: function (newAns) {
        
        if(newAns != ""){
          this.filtrate();
          this.filter_values =
          this.filter_values.filter((f_val)=>{
          return f_val.indexOf(newAns)>-1;
        });  
        }else{
          
          this.filtrate();
        }
        
      },
  }
  }
</script>
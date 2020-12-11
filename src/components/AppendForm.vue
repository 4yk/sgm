<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      persistent
      max-width="290"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="primary"
          dark
          v-bind="attrs"
          v-on="on"
        >
          Добавить
        </v-btn>
      </template>
      <v-card>
        <v-card-title class="headline">
          Добавить контакт
        </v-card-title>
        <v-card-text>
          <template>
              <v-form v-model="valid">
                <v-container>
                  <v-row>
                    <v-col
                      cols="12"
                      md="12"
                    >
                      <v-text-field
                        v-model="name"
                        :rules="nameRules"
                        :counter="50"
                        label="ФИО"
                        required
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      md="12"
                    >
                      <v-text-field
                        v-model="phone"
                        :rules="phoneRules"
                        label="Телефон"
                        required
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      md="12"
                    >
                      <v-text-field
                        v-model="dep"
                        :rules="depRules"
                        label="Отдел"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      md="12"
                    >
                      <v-text-field
                        v-model="firm"
                        :rules="firmRules"
                        label="Организация"
                        required
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-form>
            </template>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          
          <v-btn
            color="green darken-1"
            text
            @click="dialog = false"
          >
            Отмена
          </v-btn>

          <v-btn
            color="green darken-1"
            text
            @click="sendForm"
          >
            ОК
          </v-btn>


        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
  import axios from 'axios';
  export default {
    name: 'AppendForm',

    data: () => ({
      dialog: false,
      valid: false,
      name:'',
      phone: '',
      dep:'',
      firm:'',
      nameRules: [
        v => !!v || 'Введите ФИО',
        v => v.length <= 50 || 'ФИО должно содержать не больше 50 символов',
      ],
      phoneRules: [
        v => !!v || 'Введите Телефон',
        v => v.length > 1 || 'необходимо более 1 символа',
      ],
      depRules: [
        v => !!v || 'Введите Отдел',
        v => v.length > 1 || 'необходимо более 1 символа',
      
      ],
      firmRules: [
        v => !!v || 'Введите Организацию',
        v => v.length > 1 || 'необходимо более 1 символа',
    
      ],
    
    }),
    methods:{
      sendForm(){
        if(!(this.name.length < 2 || this.dep < 2 || this.firm < 2 || this.phone < 2))
          axios.post("/phones",{name:this.name,dep:this.dep,firm:this.firm,phone:this.phone},{
                          headers: {
                     
                              'Content-Type': 'application/json'
                          }
                      })
          .then((expr) => {this.$emit("pupdate", expr.data)},this.dialog = false);
      }
    }
  }
</script>
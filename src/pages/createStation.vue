<template>
    <div id="main">
        <v-form ref="form">
            <v-container>
                
            <div class="mb-4" v-for="(station, index) in stationsToCreate" :key="index">
                <v-row>
                    <v-col cols="12" md="10">
                        <v-card style="padding : 1rem;">
                            <v-text-field v-model="station.name" label="Nom de la station" required></v-text-field>

                            <v-text-field v-model="station.longitude" type="number" label="Longitude" required></v-text-field>
                            <v-text-field v-model="station.latitude" type="number" label="Lattitude" required></v-text-field>
                        </v-card>
                    </v-col>
                </v-row>
            </div>

            
            <v-btn color="success" class="mr-4" @click="validate">Créer les stations</v-btn>

            <v-btn class="mx-2" fabdark color="white" @click="addStationFromList"><v-icon>mdi-plus</v-icon></v-btn>
            <v-btn class="mx-2" fabdark color="primary" @click="removeStationFromList">
                <v-icon dark>mdi-minus</v-icon>
            </v-btn>

            <v-alert class="mt-4" type="error" v-if="errorMessage != '' ">
                {{errorMessage}}
            </v-alert>

           </v-container>
        </v-form>
    </div>
</template>

<script>
import axios from 'axios'

export default {
  name: "createStation",
  data () {
    return{
        errorMessage: '',
        stationsToCreate: [
            { name: '', longitude: 0.0, latitude: 0.0  }
        ]
    }
  },
  methods: {
      validate: function() {
          this.errorMessage = '';
          console.log(JSON.stringify(this.stationsToCreate));
          this.createStations();
      },
      createStations: function() {
            this.stationsToCreate.forEach(function(station) {
                axios.post(
                        'https://projet-web-trains.herokuapp.com/train-stations', 
                        JSON.stringify(station),
                        {
                            headers: { 
                                'Content-Type' : 'application/json' 
                            }
                        }

                );
                
            });
      },
      addStationFromList: function() {
           this.stationsToCreate.push( { name: '', longitude: 0.0, latitude: 0.0  });
      },
      removeStationFromList: function() {
          if(this.stationsToCreate.length > 1) {
              this.stationsToCreate.pop();
          } else{
                this.errorMessage = 'Vous devez ajouter au moins une station';
                window.setInterval(() => {
                    this.errorMessage = '';
                }, 10000);
          }
      }
  }
}
</script>
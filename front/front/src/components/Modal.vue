<template>

    <v-container>

        <v-card v-if="step==1">
            <v-toolbar color="indigo darken-3">
                <v-card-title >
                    Ingresar colegio al que se desea ingresar
                </v-card-title>
            </v-toolbar>
            <v-card-text>
              <v-container class="content-modal">
                <v-container class="form-container">
                    <v-form ref="form"  >
                      <!-- ingresa Comuna-->
                      <v-autocomplete
                        :items="comunas.comuna"
                        label="Comuna"
                        dense
                        filled
                        v-model="selectedComuna"
                        item-title="name"
                        item-value="value"
                      ></v-autocomplete>
                      <!-- ingresa nivel-->
                      <v-autocomplete
                        :items="niveles.niveles"
                        label=" Nivel"
                        density="compact"
                        filled
                        v-model="selectedNivel"
                        item-title="name"
                        item-value="id"
                      ></v-autocomplete>

                      <!-- ingresa colegio-->

                      <v-autocomplete
                        :items="schools"
                        label="Nombre Colegio"
                        dense
                        filled
                        v-model="selectedSchool"
                        item-title="nombre"
                        item-value="rbd"
                      ></v-autocomplete>
                      

                      <v-container  class="container-buttons">
                        <v-btn class="btn-volver" :class="{'on':!volverOn, 'off': volverOn}" color="white" rounded   @click="previusStep(), activateVolver()">
                          Volver </v-btn>

                          <v-btn class="btn-continuar" :disabled="!siguienteOn" color="blue " rounded @click="nextStep(), activateVolver(), getProbability()">
                          Continuar</v-btn>
                      </v-container >
                    </v-form>
                  </v-container >
              </v-container >
            </v-card-text>
          </v-card>

          <v-card v-if="step==2 && probability == -1"> <!--cerrillos basica 4to medio-->
            <v-toolbar color="indigo darken-3">
                <v-card-title >
                  Lo lamentamos
                </v-card-title>
            </v-toolbar>
            <v-card-text>
              <v-container class="content-modal">
                <h1>No se puede calcular la probabilidad</h1>
                    <v-container  class="container-buttons-p-1">
                      <v-card-actions class="container-buttons">
                        <v-container class="container-continuar" >
                      <v-btn class="btn-continuar-p-1" :disabled="!siguienteOn" color="blue " rounded @click="nextStep(), activateVolver(), getProbability(), resetForm()">
                        Continuar</v-btn>
                      </v-container>
                      <v-container class="container-volver">
                        <v-btn class="btn-volver" :class="{'on':!volverOn, 'off': volverOn}" color="white" rounded   @click="previusStep(), activateVolver()">
                        Volver </v-btn>
                      </v-container>
                      </v-card-actions>
                    </v-container>
                </v-container >
            </v-card-text>
          </v-card>


          <v-card v-if="step==2  && probability <= 0.5 && probability != -1">
            <v-toolbar color="indigo darken-3">
                <v-card-title >
                  El colegio seleccionado tiene una probabilidad de completar los cupos de:
                </v-card-title>
            </v-toolbar>
            <v-card-text>
              <v-container class="content-modal">
                <h1>tienes una probabilidad de {{probability*100}}%</h1>
                <p>Esto significa que el colegio cuenta con los cupos suficientes para aceptar un nuevo alumno</p>
                <br>
                <p>Por lo tanto es muy probable que ingrese a este establecimiento</p>
                    <v-container  class="container-buttons-p-1">
                      <v-card-actions class="container-buttons">
                        <v-container class="container-continuar" >
                      <v-btn class="btn-continuar-p-1" :disabled="!siguienteOn" color="blue " rounded @click="nextStep(), activateVolver(), getProbability(), resetForm()">
                        Continuar</v-btn>
                      </v-container>
                      <v-container class="container-volver">
                        <v-btn class="btn-volver" :class="{'on':!volverOn, 'off': volverOn}" color="white" rounded   @click="previusStep(), activateVolver()">
                        Volver </v-btn>
                      </v-container>
                      </v-card-actions>
                    </v-container>
                </v-container >
            </v-card-text>
          </v-card>

          <!-- preguntas sae colegio-->
          <v-card v-if="step==2 && probability > 0.5 && probability != -1">
            <v-toolbar color="indigo darken-3">
                <v-card-title >
                    Preguntas SAE
                </v-card-title>
            </v-toolbar>
            <v-card-text>
              <v-container class="content-modal">
                <v-container class="form-container">
                  <v-form ref="form2">
                    <v-checkbox
                      v-model="hermanos"
                      label="Tiene hermanos en el colegio?"
                      color="info"
                      :items="preguntas"
                      hide-details
                    ></v-checkbox>
                    <v-checkbox
                      v-model="prioritario"
                      label="Es estudiante prioritario?"
                      color="info"
                      :items="preguntas"
                      hide-details
                    ></v-checkbox>
                    <v-checkbox
                      v-model="hijo"
                      label="Es hijo de funcionario?"
                      color="info"
                      :items="preguntas"
                      hide-details
                    ></v-checkbox>
                    <v-checkbox
                      v-model="exalumno"
                      label="Ex alumno del colegio?"
                      color="info"
                      :items="preguntas"
                      hide-details
                    ></v-checkbox>
                    <v-container class="container-container-butons" >
                      <v-card-actions class="container-buttons">
                        <v-container class="container-continuar" >
                          <v-btn class="btn-continuar-step-2" color="white" rounded @click="nextStep(), activateVolver(), getProbabilitySae()"
                          >Continuar</v-btn>
                        </v-container>

                        <v-container class="container-volver">
                          <v-btn class="btn-volver" :class="{'on':!volverOn, 'off': volverOn}" color="white" rounded   @click="previusStep(), activateVolver()"
                          >Volver </v-btn>
                        </v-container>
                      </v-card-actions>
                    </v-container>
                  </v-form>
                  </v-container >
              </v-container >
            </v-card-text>
          </v-card>


          <v-card v-if="step==3  && probabilitySAE > 0.5 && probabilitySAE != -1"><!--cerrillos bicentenario 8tavo-->
            
            <v-toolbar color="indigo darken-3">
                <v-card-title >
                  En el colegio seleccionado tienes una probabilidad de ingresar de:
                </v-card-title>
            </v-toolbar>
            <v-card-text>
              <v-container class="content-modal">
                <h1>tienes una probabilidad de {{probabilitySAE*100}}% </h1>
                <p class="info-cupos">Esto significa que bajo las condiciones SAE que existe alta probabilidad de quedar en el colegio</p>
                    <v-container  class="container-buttons-p-1">
                      <v-card-actions class="container-buttons">
                        <v-container class="container-continuar" >
                      <v-btn class="btn-continuar-inicio" color="white " rounded @click="volverInicio()">
                        Inicio</v-btn>
                      </v-container>
                      <v-container class="container-volver">
                        <v-btn class="btn-volver" :class="{'on':!volverOn, 'off': volverOn}" color="white" rounded   @click="previusStep(), activateVolver()">
                        Volver </v-btn>
                      </v-container>
                      </v-card-actions>
                    </v-container>
                </v-container >
            </v-card-text>
          </v-card>

          <v-card v-if="step==3  && probabilitySAE <= 0.5 && probabilitySAE != -1"><!--cerrillos bicentenario 8tavo-->
            <v-toolbar color="indigo darken-3">
                <v-card-title >
                  En el colegio seleccionado tienes una probabilidad de ingresar de:
                </v-card-title>
            </v-toolbar>
            <v-card-text>
              <v-container class="content-modal">
                <h1>tienes una probabilidad de {{probabilitySAE*100}}%</h1>
                <p class="info-cupos">Esto significa que bajo las condiciones SAE que existe baja probabilidad de quedar en el colegio</p>
                    <v-container  class="container-buttons-p-1">
                      <v-card-actions class="container-buttons">
                        <v-container class="container-continuar" >
                      <v-btn class="btn-continuar-inicio" color="white " rounded @click="nextStep(), activateVolver(), getSimilarSchools()">
                        Recomiendame un colegio</v-btn>
                      </v-container>
                      <v-container class="container-volver">
                        <v-btn class="btn-volver" :class="{'on':!volverOn, 'off': volverOn}" color="white" rounded   @click="previusStep(), activateVolver()">
                        Volver </v-btn>
                      </v-container>
                      </v-card-actions>
                    </v-container>
                </v-container >
            </v-card-text>
          </v-card>

          <v-table v-if="step==4">
            <thead>
              <tr>
                <th class="text-left">
                  Nombre Colegio
                </th>
                <th class="text-left">
                  Calle
                </th>
                <th class="text-left">
                  Similaridad
                </th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="item in recomendedSchools.schools" 
                :key="item.name"
              > <!-- cambiar por lo que viene de la api-->
                <td>{{ item.name }}</td>
                <td>{{ item.street }}</td>
                <td>{{ item.similarity*100 }}%</td>
              </tr>
            </tbody>
  </v-table>
    </v-container>
</template>


<script>

// Components

import comunas from '../utils/comunas.json'
import niveles from '../utils/niveles.json'
import schoolsimilares from '../utils/schoolsimilar.json'

export default {
  created(){
    this.$watch(
      () => this.selectedComuna,
      () => {
        this.fetchData()
      },
      // fetch the data when the view is created and the data is
      // already being observed
      { immediate: true }
    ),
    this.$watch(
      () => this.selectedNivel,
      () => {
        this.fetchData()
      },
      // fetch the data when the view is created and the data is
      // already being observed
      { immediate: true }
    ),
    this.$watch(
      () => this.selectedSchool,
      () => {
        this.fetchData()
      },
      // fetch the data when the view is created and the data is
      // already being observed
      { immediate: true }
    )
  },

  data(){
    return{
      step:1,
      preguntas:['Si','No'],
      volverOn:true,
      siguienteOn:false,
      schools:[],
      comunas,
      niveles,
      loading_schools:false,
      selectedSchool:"",
      selectedComuna:"",
      selectedNivel:"",
      hermanos:"",
      prioritario:"",
      hijo:"",
      exalumno:"",
      probability:'',
      probabilitySAE:0.4,
      recomendedSchools:[],
      schoolsimilares
    }
    
  },
  methods:{
    nextStep(){
      if(this.step<4){
        this.step ++
       
      }
    },
    previusStep(){
      if(this.step>1 && this.step<=4){
        this.step --
      }
    },
    activateVolver(){
        if(this.step > 1){
          this.volverOn = false
        }else{
          this.volverOn =true
        }

      },
      async getProbability(){
        if(this.selectedComuna && this.selectedNivel && this.selectedSchool){
          //let response = await fetch(`https://pokeapi.co/api/v2/pokemon/ditto`)
          
          let response = await fetch(`http://172.20.1.4/getProbability/${this.selectedSchool}/${this.selectedNivel}`);
          let data = await response.json()
          this.probability = data.probability
          setTimeout(function(){}, 2000);
          //fetch(`/getProbability/${this.selectedSchool}/${this.selectedNivel}`)
        //.then(response => response.json())
        //.then(data => this.schools=data)
         //

      }else{
        console.log('no hay nada')

      }

      },
      async getProbabilitySae(){     
      
  const rawResponse = await fetch('http://172.20.1.4/getProbabilitySecondStage/', {
    method: 'POST',
    headers: {
      'Accept': 'application/json',
      'Content-Type': 'application/json',
      'Access-Control-Allow-Headers': '*'
    },
    body: JSON.stringify({id_school: this.selectedSchool , id_level: this.selectedNivel, sibilingsInSchool:this.hermanos, isPriority:this.prioritario, isSonOfTeacher: this.hijo, isExStudent:this.exalumno })
  });
  const content = await rawResponse.json();

  console.log(content);
},

      async getSimilarSchools(){
        const comunas = this.comunas.comuna
        let codigo_comuna = 0
        comunas.forEach(e=>{
          if(e.value == this.selectedComuna){
            console.log(e.cod_comuna)
            codigo_comuna = e.cod_comuna
          }
        })
        let response = await fetch(`http://172.20.1.4/getSimilarSchools/${this.selectedSchool}/${this.selectedNivel}/${codigo_comuna}`);
          let data = await response.json()
          this.recomendedSchools = data
          console.log(this.recomendedSchools)

          //${this.selectedComuna}
          //13126
        

      },
      fetchData() {
        if(this.selectedComuna){
        fetch(`https://apisae.mineduc.cl/sae-api-vitrina/v1/establecimientos?comuna=${this.selectedComuna}`)
        .then(response => response.json())
        .then(data => this.schools=data)
        
        }
        if(this.selectedComuna && this.selectedNivel && this.selectedSchool){
          this.siguienteOn =true

        }else{
          this.siguienteOn =false
        }
      },
      resetForm(){
        this.$refs.form.reset()
      },
      volverInicio(){
        this.step=1
        this.volverOn =true
        this.selectedComuna =''
        this.selectedNivel =''
        this.selectedSchool=''
      }

  }
};
</script>

<style>

.btn-continuar{
  background-color: #2196F3
}

.btn-volver{
    background-color: #2196F3 !important;

    
}


.off{
  display: none;
}

.on{
  background-color: #2196F3 !important;

}

.container-buttons{
  display:flex;
  justify-content: space-between;
  flex-direction: row-reverse;
  width: 100%;
}



.btn-continuar-step-2{
  background-color: #2196F3;
  float: right;

}

.container-container-butons{
  padding: 0px !important;
}

.continuar-off{
  pointer-events: none;
  background-color: red !important;
}


.btn-continuar-p-1{
  display: none;
}

.container-buttons-p-1{
  display:flex;
  justify-content: space-between;
  width: 100%;

}

.btn-continuar-inicio{
  background-color: #2196F3;
  float: right;
}

</style>
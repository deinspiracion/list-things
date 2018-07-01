<template>
  <v-card flat>


    <v-layout row pb-2>
      <v-flex xs8 offset-xs2>
        <v-card class="card--flex-toolbar">
          <v-toolbar card prominent>
            <v-toolbar-title class="body-2 grey--text">{{nombre}}</v-toolbar-title>

            <v-spacer></v-spacer>

            <v-btn icon>
              <v-icon>search</v-icon>
            </v-btn>

            <v-btn icon>
              <v-icon>apps</v-icon>
            </v-btn>

            <v-btn icon>
              <v-icon>more_vert</v-icon>
            </v-btn>
          </v-toolbar>

          <v-divider></v-divider>

          <v-card-text style="">
            <v-container grid-list-md text-xs-center>
              <v-layout row wrap>

                <v-flex xs10>
                  <v-text-field
                    v-model="tarea" 
                    label="Ingresa tu tarea"
                  >
                  </v-text-field>
                </v-flex>

                <v-flex xs2 > 
                       <v-btn v-on:click="addTask" outline fab color="primary">
                        <v-icon>add</v-icon>

                      </v-btn>
                </v-flex>

              </v-layout>
            </v-container>
            <div v-if="tsk.length == 0">No existen Tareas</div>

            <v-layout row wrap="">

              <template  v-for="t in tsk">
                
                <v-flex  xs10>

                <v-list subheader>
                      <template>
                        <v-list-tile ripple>{{t}}</v-list-tile>
                        <v-divider></v-divider>
                      </template>
                  </v-list>


                </v-flex>
                <v-flex  xs2>
                  <v-btn v-on:click="checkTask" icon outline color="green">
                      <v-icon>check</v-icon>
                  </v-btn>

                    <v-btn v-on:click="deleteTask" icon outline color="error">
                      <v-icon>remove</v-icon>
                  </v-btn>
                  
                </v-flex>

              </template>



            </v-layout>
                    

                  
            





         

          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </v-card>
</template>

<script>
export default {
  name: 'List',
  props: {
    msg: String
  },
  data (){
    return {
      nombre: "lista de tareas de hoy",
      tarea: "",
      tsk: this.getTask()

    }
  },
  methods: {
    getTask() {
      let taskLocal = JSON.parse(localStorage.getItem('task')) || []
      
      return taskLocal
    },
    addTask(){
      if(this.tarea.trim() ==""){
        return;
      }
      if(!localStorage.getItem('task')){
        
        localStorage.setItem('task','[]')
      }else{
       let taskString= JSON.parse(localStorage.getItem('task'))
       taskString.unshift(this.tarea)
       localStorage.setItem('task',JSON.stringify(taskString))
       this.tsk = taskString
      }
      

      

    },
    checkTask() {},
    deleteTask() {
}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

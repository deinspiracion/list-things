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
            <div v-if="tsk.length == 0" class="contentIconNoList">
              <v-icon class="iconNoList">view_list</v-icon>
              <p>Sin tareas Pendientes</p>
            </div>

            <v-layout row wrap="">

              <template  v-for="(t,index) in tsk">
                
                <v-flex  xs10>

                <v-list subheader>
                      <template>
                        <v-list-tile ripple>{{t}}</v-list-tile>
                        <v-divider></v-divider>
                      </template>
                  </v-list>


                </v-flex>
                <v-flex  xs2>
                  <v-btn v-on:click="checkTask(index)" icon outline color="green">
                      <v-icon>check</v-icon>
                  </v-btn>

                    <v-btn v-on:click="deleteTask(index)" icon outline color="error">
                      <v-icon>remove</v-icon>
                  </v-btn>
                  
                </v-flex>

              </template>



            </v-layout>
                    
               
                 <div v-if="tskSuccess.length > 0" class="">
                   <v-divider></v-divider>
                  <p class="titleTaskSuccess">Tareas terminadas</p>

                </div>
             <v-layout row wrap="">

              <template  v-for="(t,index) in tskSuccess">
                
                <v-flex  xs10>

                <v-list subheader>
                      <template>
                        <v-list-tile ripple class="listSuccess">{{t}}</v-list-tile>
                        <v-divider></v-divider>
                      </template>
                  </v-list>


                </v-flex>
                <v-flex  xs2>
                 <v-btn v-on:click="deleteTaskSuccess(index)" icon outline color="error">
                      <v-icon>delete_forever</v-icon>
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
      tsk: this.getTask(),
      tskSuccess:this.getTaskSuccess()

    }
  },
  methods: {
    getTask() {
      let taskLocal = JSON.parse(localStorage.getItem('task')) || []
      return taskLocal
    },
    getTaskSuccess() {
      
      let taskSuccessLocal = JSON.parse(localStorage.getItem('taskSuccess')) || []
      console.log(taskSuccessLocal);
      
      return taskSuccessLocal
    },
    addTask(){
      if(this.tarea.trim() ==""){
        return;
      }
      if(!localStorage.getItem('task')){
        localStorage.setItem('task','[]')
      }
       let taskString= JSON.parse(localStorage.getItem('task'))
       taskString.unshift(this.tarea)
       localStorage.setItem('task',JSON.stringify(taskString))
       this.tsk = taskString
       this.tarea= ''
    },
    checkTask(index) {
      if(!localStorage.getItem('taskSuccess')){
        localStorage.setItem('taskSuccess','[]')
      }
      let taskSuccessString= JSON.parse(localStorage.getItem('taskSuccess'))
      taskSuccessString.unshift(this.tsk[index])
      localStorage.setItem('taskSuccess',JSON.stringify(taskSuccessString))
      this.tskSuccess = taskSuccessString
      this.tsk.splice(index,1)
      localStorage.setItem('task',JSON.stringify(this.tsk))

    },
    deleteTask(index) {
      this.tsk.splice(index,1)
      localStorage.setItem('task',JSON.stringify(this.tsk))
    },
    deleteTaskSuccess(index) {
      this.tskSuccess.splice(index,1)
      localStorage.setItem('taskSuccess',JSON.stringify(this.tskSuccess))
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.contentIconNoList{
  display: flex;
  justify-content: center;
  flex-direction: column;
  text-align: center;
  color: #c0bcbc;;
}
.iconNoList{
  font-size: 200px;
  text-align: center;
  color: #c0bcbc;
}
.titleTaskSuccess{
  font-size: 25px;
  text-align: center;
  margin-top: 20px;
}
.listSuccess{
  text-decoration: line-through;
  background-color: #e9e9e9;
border-radius: 50px;
}
</style>

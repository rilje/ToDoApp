<template>
  <br>
  <br>
  <div class="border container col-md-5  h-100">
    <br>
    <h1 class="text-center">ToDo App</h1>
    <div class="mb-3">
      <label for="exampleFormControlInput1" class="form-label"><h4>New ToDo </h4></label>
      <input v-model="task" type="text" class="form-control unos" id="exampleFormControlInput1" placeholder="">
    </div>
    <div class="text-center">
      <button @click="dodajTask" class="btn btn-primary col-md-12 dugmee">Add ToDo</button>
    </div>
    <br>
    <h3>ToDo List</h3>
    <hr>
    <div class="col-md-12  taskovi">
      <h5 v-if="taskovi.length <= 0" class="text-center">Empty list</h5>
      <div v-if="!filter" v-for="task,index in this.taskovi" :key="index" class="border d-flex justify-content-between p-2 m-2">
        <div class="zaTask">
          {{ index + 1 }}. {{ task }}
        </div>
        <div>
          <button @click="obrisiTask(index)" class="btn btn-danger">Remove</button>
        </div>
      </div>  
      <br>
      <h5 v-if="filter">Completed Tasks</h5>
      
      <div v-if="filter" v-for="task,index in this.zavrseniTaskovi" :key="index" class="border d-flex justify-content-between p-2 m-2">
        
        <div class="zaTask">
          {{ index + 1 }}. {{ task }}
        </div>
        <div>
          <button @click="obrisiZavrsenTask(index)" class="btn btn-danger">Remove</button>
        </div>
      </div>
      <hr>
      <div class="d-flex justify-content-between">
        <div class="d-flex gap-1 p-2">
          <span><button @click="ponistiFilter" class="btn btn-secondary">Active</button></span>
          <span><button @click="primeniFilter" class="btn btn-secondary">Completed</button></span>
        </div>
        <div v-if="filter" class="p-1">
          <button @click="obrisiZavrsene" class="btn btn-secondary" :class="{disabled : zavrseniTaskovi.length == 0}">Clear history</button>
        </div>
      </div>
    </div>
  </div>
    
</template>
<script>
export default {
  data() {
    return {
      task: "",
      taskovi: [],
      zavrseniTaskovi: [],
      filter: false
    }
  },
  methods: {
    dodajTask(){
      console.log('dodajem')
      if(this.task == ''){
        return
      }
      this.taskovi.push(this.task)
      localStorage.setItem('taskovi',JSON.stringify(this.taskovi))
      this.task = ""
    },
    obrisiTask(index){
      this.zavrseniTaskovi.push(this.taskovi[index])
      localStorage.setItem('Completed-Tasks', JSON.stringify(this.zavrseniTaskovi))
      this.taskovi.splice(index,1)
      localStorage.setItem('taskovi',JSON.stringify(this.taskovi))
    },
    dohvatiPodatke(){
      this.taskovi = JSON.parse(localStorage.getItem('taskovi'))
      this.zavrseniTaskovi = JSON.parse(localStorage.getItem('Completed-Tasks'))
    },
    inicirajLocalStorage(){
      if(!JSON.parse(localStorage.getItem('taskovi'))){
        localStorage.setItem('taskovi', JSON.stringify([]))
      }
      if(!JSON.parse(localStorage.getItem('Completed-Tasks'))){
        localStorage.setItem('Completed-Tasks', JSON.stringify([]))
      }
    },  
    primeniFilter(){
      this.filter = true
    },
    obrisiZavrsenTask(index){
      this.zavrseniTaskovi.splice(index,1)
      localStorage.setItem('Completed-Tasks',JSON.stringify(this.zavrseniTaskovi))
    },
    ponistiFilter(){
      this.filter = false
    },
    obrisiZavrsene(){
      this.zavrseniTaskovi = []
      localStorage.setItem('Completed-Tasks', JSON.stringify(this.zavrseniTaskovi))
    }

  },
  mounted() {
    this.inicirajLocalStorage()
    this.dohvatiPodatke()
  },
}
</script>
<style scoped>
  .dugmee{
    height: 60px;
    font-size: large;
  }
  .taskovi{
    margin: 0 auto;
  }
  .zaTask{
    font-size: 1.3em;
  }
  .unos{
    height: 60px;
  }
</style>
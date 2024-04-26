<script setup>
import { vAutoAnimate } from '@formkit/auto-animate'
</script>

<template>
  <body>
    <h1>Хранитель имён</h1>
    <div v-auto-animate class="container">

      <section>
        <input type="text" class="input-group" v-model="inputName" placeholder="Введите имя" @keyup.enter="addName">
        <button @click="addName" class="btn btn-success">Добавить</button>
      </section>

      <ul class="list-group" v-for="name in nameList" :key="name.uid">
        <li class="list-group-item">
          {{ name }}
          <button class="btn btn-danger" @click="() => deleteName(name)">
            Удалить
          </button>
        </li>
      </ul>

    </div>
  </body>
</template>

<script>

export default{
  name : 'App',
  
  computed: {
    filteredNames() {
      const { names, filterId } = this
      return Object.values(names)
        .filter(name => parseInt(name.pubdate.substring(0,4)) >= filterId)
    }
  },



  data(){
    return{
      inputName: '',
      nameList: [''],
    }
  },

  methods:{
    addName(){
      this.nameList.push(this.inputName)
      this.inputName = ""
      localStorage.setItem('NameList',JSON.stringify(this.nameList))
    },


    deleteName(name){
      const index = this.nameList.indexOf(name)
      if(index > -1){
        this.nameList.splice(index, 1)
      }
      localStorage.setItem('NameList',JSON.stringify(this.nameList))
    },
  },
  
  mounted(){
    const data = localStorage.getItem('NameList');
    if (data === null) return;
    this.nameList = JSON.parse(data)
    },
  
  components:{

  }
}
</script>

<style>
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background-color: #333333;
    margin: 0;
    padding: 0;
}

.container {
    background-color: #444444;
    max-width: 700px;
    margin: 20px auto;
    padding: 15px;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 236, 91, 0.979);
}

h1 {
    font-size: 30px;
    color: #fff;
}

input[type="text"] {
    width: 70%;
    padding: 8px;
    border-radius: 5px;
}

button {
    padding: 10px 20px;
    border-radius: 5px;
    background-color: #007BFF;
    color: #fff;
    border: none;
    cursor: pointer;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 8px 0;
    color: whitesmoke
}

.btn-danger {
    padding: 8px;
    border-radius: 10%;
    background-color: #ff0000;
    color: rgb(255, 255, 255);
    cursor: pointer;
}
</style>

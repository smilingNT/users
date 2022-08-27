<template>
  <div id="app">
    <Header></Header>
   
    <div class="container">
      <MyHeader :addUser="addUser" :userInformation="userInformation" 
      :search="search" />
      <List :userInformation="userInformation"
            :deleteUser="deleteUser"
            :searchResult="searchResult"></List>
    </div>
    
      
    </div>

</template>

<script>
import MyHeader from "./components/MyHeader";
import List from "./components/List.vue";

import Header from './components/Header';


export default {
  name: "App",
  components:{
    List,
  
    MyHeader,
    Header,
  
  },
  data() {
    return {
     
      // userInfo:JSON.parse(localStorage.getItem('userInformation')) || [],
      searchResult:JSON.parse(localStorage.getItem('userInformation')) || [],
    }
  },
  methods:{
    addUser(person){
      this.searchResult.unshift(person)
      console.log(this.searchResult);
      this.userInformation.unshift(person)
      
      localStorage.setItem("userInformation", JSON.stringify(this.userInformation))
    },
      deleteUser(id){
      this.searchResult = this.searchResult.filter(person => person.id !== id);
      this.userInformation = this.userInformation.filter(person => person.id !== id);
      
    },
    search(keyword){
        this.searchResult=this.userInformation.filter(person => {
          if(keyword=="") return true
          return person.username.match(keyword)
        })
        console.log(this.searchResult)
    }
  
  },
  computed:{
    userInformation:{
      get(){
          return JSON.parse(localStorage.getItem('userInformation'))
      },
      set(newValue) {
        //本地存储存的是key和value都是字符串
        //数据存放在本地存储中
        localStorage.setItem("userInformation", JSON.stringify(newValue))
        console.log(this.userInformation)
      }
    },

  },
  
}
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.container {
  width: 80%;
  margin: 0 auto;
  overflow: auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

</style>



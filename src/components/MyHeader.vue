<template>
  <div class="users-header">
    <button @click="showAddDialog">新建</button>
    <input type="text" placeholder="按关键字搜索" v-model="keyword" @keyup.enter="searchR"/>
    <button class="right_btn">撤销</button>

    <div class="dialog">
      <el-dialog title="新建/编辑用户" :visible.sync="addDialogVisible" @close="closeAddForm">
    <div class="adduserinfo">
      <div class="item"><label>姓名</label><input type="text"  v-model="username"></div>
      <div class="item"><label>性别</label><select name="sex" v-model="sex">
        <option value="男">男</option>
        <option value="女">女</option>
        </select></div>
      <div class="item"><label>年龄</label><input type="number"  v-model="age"></div>
      <div class="item"><label>联系电话</label><input type="text"  v-model="telNum"></div>
    </div>
    <div>
      <label class="magr">详细地址</label>
    <div class="block">
        <el-cascader
        class="magr"
    v-model="address1"
    :options="cityData"
    @change="handleChange"></el-cascader>
     <input type="text" placeholder="请输入详细地址" v-model="address2" />
</div>
 
    </div>
  <div slot="footer" class="dialog-footer">
    <el-button @click="closeAddForm">取 消</el-button>
    <el-button type="primary" @click="addForm">确 定</el-button>
  </div>
</el-dialog>
</div>
  </div>
</template>

<script>
import cityData from './citydata.js'
import {nanoid} from 'nanoid'

export default {
  props:["addUser","search"],
  data() {
      return {
        cityData,
        username:"",
        sex:"",
        telNum:null,
       
        address2:"",
        address1:[],
       keyword:"",
       addDialogVisible:false ,
        formLabelWidth: '20px',
        age:null,
        selected:false
       
      };
    },
   
  methods: {
    showAddDialog(){
      this.addDialogVisible=true;
    },
    addForm(){
      this.addDialogVisible=false;
      var person = {username:this.username,age:this.age,sex:this.sex,telNum:this.telNum,address:this.address,id:nanoid(),selected:this.selected,
      address1:this.address1,address2:this.address2};
      this.addUser(person);
      this.username = undefined;
      this.age=undefined;
      this.sex=undefined;
      this.telNum=undefined;
      this.address1=undefined;
      this.address2=undefined;
      
      
    },
    closeAddForm(){
      this.addDialogVisible=false;
      this.username = undefined;
      this.age=undefined;
      this.sex=undefined;
      this.telNum=undefined;
      this.address1=undefined;
      this.address2=undefined;

    },
    handleChange(value){
      console.log(value)
      // this.address1= value.join("");
      // console.log(this.address1)
    },
    searchR (){
        this.search(this.keyword.trim())
      
    }
 
},
computed:{
  address(){
    return this.address1.join("")+this.address2;
  },
  
  }
}

</script>

<style scoped>
/*header*/
.magr{
  margin-left:5%;
}
select{
  width: 60%;
 
}
.block{
  display: flex;
}
.block input{
  margin-left: 10px;
}
.dialog{

  overflow: auto;
}
.adduserinfo{
  overflow: auto;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
 
  align-content: flex-start;
  
}
.item{
  width: 40%;
  margin: auto;
}
.item:nth-child(3) {
  align-self: flex-start;
}
.item label,input{
  display: block;
}
.item input{
  margin-left: 0px;
}
.users-header{
  display: flex;
  vertical-align: center;
}
 .right_btn{
 margin-left: auto;
}
.users-header > input {
  width: 200px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 3px 7px;
  margin-left: 10px;
}

.users-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
}

</style>

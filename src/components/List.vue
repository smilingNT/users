<template>
<div>
  <el-table
    :data="userInfo"
    style="width: 100%">
    <el-table-column
      label=""
      width="50">
      <template slot-scope="scope">
       
        <span style="margin-left: 10px"><input type="checkbox" :checked="scope.row.selected" @change="handleCheck(scope.row.id)"/></span>
      </template>
    </el-table-column>
    <el-table-column
      label="姓名"
      width="120">
      <template slot-scope="scope">
        
        <span style="margin-left: 10px">{{ scope.row.username }}</span>
      </template>
    </el-table-column>
     <el-table-column
      label="年龄"
      width="120">
      <template slot-scope="scope">
        
        <span style="margin-left: 10px">{{ scope.row.age }}</span>
      </template>
    </el-table-column>
     <el-table-column
      label="性别"
      width="120">
      <template slot-scope="scope">
        
        <span style="margin-left: 10px">{{ scope.row.sex}}</span>
      </template>
    </el-table-column>
     <el-table-column
      label="联系电话"
      width="120">
      <template slot-scope="scope">
        
        <span style="margin-left: 10px">{{ scope.row.telNum }}</span>
      </template>
    </el-table-column>
    <el-table-column
      label="详细地址"
      width="500">
      <template slot-scope="scope">
      
         <span style="margin-left: 10px">{{ scope.row.address }}</span>
      </template>
    </el-table-column>
    <el-table-column label="操作">
      <template slot-scope="scope">
        <el-button
          size="mini"
          @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
        <el-button
          size="mini"
          
          @click="handleDelete(scope.$index, scope.row)">删除</el-button>
      </template>
    </el-table-column>
   
  </el-table>
 <!-- <button class="Batch_delete_btn">批量删除</button> -->
 <el-button size="mini" class="b_btn" @click="deleteMany">批量删除</el-button>
  <el-dialog title="新建/编辑用户" :visible.sync="editDialogVisible" @close="closeEidtForm">
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
   ></el-cascader>
     <input type="text" placeholder="请输入详细地址" v-model="address2" />
</div>
 
    </div>
  <div slot="footer" class="dialog-footer">
    <el-button @click="closeEidtForm">取 消</el-button>
    <el-button type="primary" @click="Edit">确 定</el-button>
  </div>
</el-dialog>

</div>
    
</template>

<script>
import cityData from './citydata.js'

  export default {
    name: "List",
    props:["userInformation","deleteUser","searchResult"],
    data() {
      return {
         editDialogVisible:false ,
       
         cityData,
        username:"",
        sex:"",
        telNum:null,
        address2:"",
        address1:[],
        age:null,
        formLabelWidth: '20px',
        editId:null
        
      }
    },
    created(){

      this.getUserInfo()
    },
    computed:{
        userInfo:{
          get(){
            return this.searchResult
          },
          set(value){
            this.userInfo=value
          }
        }
    },
    methods: {
      closeEidtForm(){
         this.editDialogVisible=false
      },

      getUserInfo(){
        console.log(this.userInfo)
      },

      handleEdit(index, row) {
        this.editDialogVisible=true;
        console.log(index, row);
        this.username = row.username;
        this.age=row.age;
        this.sex=row.sex;
        this.telNum=row.telNum;
        this.address1=row.address1;
        this.address2=row.address2;
        this.editId=row.id
        console.log(row.id)
        
        
   
        
      },
      Edit(){
        this.userInfo.forEach(user => 
        { if(user.id==this.editId)
          {
            user.username=this.username;
            user.age=    this.age
            user.sex =    this.sex;
            user.telNub=this.telNum;
            user.address1 =this.address1
            user.address2 = this.address2
          }
      })
      this.editDialogVisible=false
      },
      handleDelete(index, row) {
       this.deleteUser(row.id)
       console.log("1111")
      
      },
      handleCheck(id){
        this.userInfo.forEach(person =>
        {
          if(person.id==id)
          person.selected=!person.selected
        }
        )
      },
      deleteMany(){
        var users = this.userInfo.filter(person => {
          return !person.selected
        })
        var deleteusers = this.userInfo.filter(person => {
          return person.selected
        })
        deleteusers.forEach(person =>
             this.deleteUser(person.id)
         )
        localStorage.setItem("userInformation", JSON.stringify(users))
        console.log("删除了")
   
      
      }
    
      
    },
    watch:{
       userInfo:{
                deep: true, //监视多级结构的属性
                handler(newValue){
                  localStorage.setItem("userInformation", JSON.stringify(newValue)) 
                   
                }
            }
    }
  }
</script>

<style scoped >
.el-table >>> .el-table__cell{
  text-align: center;
}
.magr{
  margin-left:5%;
}
.b_btn{
  margin-top: 10px;
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
<template>
  <div class="header">
    {{title}}
  </div>
</template>

<script>
export default {
  //注意不管是你写的data也还还是methods也好，甚至是computed计算属性也好都会出现在组件事例对象vc身上
  //属性值不能重名
  name: "MyHeader",
  data(){
    return {
      title: '用户管理'
    }
  },
  
}
</script>

<style scoped>
/*header*/


.header  {
  widows: 100%;
  height: 100px;
  margin-bottom: 20px;
  text-align:center; 
  line-height:100px;
  background-color:rgb(222, 212, 201);
  font-size: 20px;
}
</style>

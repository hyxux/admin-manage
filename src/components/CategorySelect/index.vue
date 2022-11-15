<template>
  <div>
    <!-- inline:代表行内表单,代表一行可以放多个表单元素 -->
    <el-form :inline="true"  class="demo-form-inline" :model="cForm">
      
      <el-form-item label="一级分类" >
        <el-select :disabled='show'  placeholder="请选择" v-model="cForm.category1Id" @change="handler1">
          <el-option :label="c1.name" :value="c1.id" v-for="(c1) in list1" :key="c1.id"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="二级分类">
        <el-select  :disabled='show' placeholder="请选择" v-model="cForm.category2Id" @change="handler2">
          <el-option :label="c2.name" :value="c2.id" v-for="(c2) in list2" :key="c2.id"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="三级分类">
        <el-select  :disabled='show' placeholder="请选择" v-model="cForm.category3Id" @change="handler3">
          <el-option :label="c3.name" :value="c3.id" v-for="(c3) in list3" :key="c3.id"></el-option>
        </el-select>
      </el-form-item>
      
    </el-form>
  </div>
</template>

<script>
export default {
  name: "CategorySelect",
  data() {
      return {
        list1:[],
        cForm:{
            category1Id:'',
            category2Id:'',
            category3Id:'',
        },
        list2:[],
        list3:[],
      }
    },
    props:['show'],
    //组件挂载完毕 向服务器发请求
    mounted(){
      //获取一级分类的数据方法
      this.getCategoryList()
    },
    methods:{
      async getCategoryList(){
        //获取一级分类请求
        let result=await this.$API.attr.reqCategory1List();
        if(result.code==200){
          this.list1=result.data
        }
      },
      //一级分类select事件回调(当一级分类option发送变化获取二级分类)
      async handler1(){
        //清除数据
        this.list2=[]
        this.list3=[]
        this.cForm.category2Id=''
        this.cForm.category3Id=''
        const {category1Id}=this.cForm
        let result= await this.$API.attr.reqCategory2List(category1Id)
        if(result.code==200){
            this.list2=result.data
        }
      },
      async handler2(){
        //清除数据
        this.list3=[]
        this.cForm.category3Id=''
        const {category2Id}=this.cForm
        let result= await this.$API.attr.reqCategory3List(category2Id)
        if(result.code==200){
            this.list3=result.data
        }
      },
      async handler3(){
        const {category1Id,category2Id,category3Id}=this.cForm
        this.$emit('getCategoryId',{category1Id,category2Id,category3Id})
      }
    }
};
</script>

<style>
</style>
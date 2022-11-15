<template>
  <div>
    <el-button
      type="primary"
      icon="el-icon-plus"
      style="margin: 10px 0px"
      @click="showDialog"
      >添加</el-button
    >
    <!-- 
      表格组件
       -->
    <el-table :data="list" border style="width: 100%">
      <el-table-column type="index" label="序号" width="80px" align="center">
      </el-table-column>
      <el-table-column prop="tmName" label="品牌名称" width="width">
      </el-table-column>
      <el-table-column prop="logoUrl" label="品牌logo" width="width">
        <template slot-scope="{ row }">
          <img :src="row.logoUrl" alt="" style="width: 100px; height: 100px" />
        </template>
      </el-table-column>
      <el-table-column prop="prop" label="操作" width="width">
        <template slot-scope="{ row }">
        <el-button
          type="warning"
          icon="el-icon-edit"
          size="mini"
          @click="updateTradeMark(row)"
          >修改</el-button
        >
        <el-button type="danger" icon="el-icon-delete" @click="deleteTradeMark(row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 分页器
    
          @size-change="handleSizeChange"
      @current-change="handleCurrentChange" -->
    <el-pagination
      style="margin-top: 20px; textAlign: center"
      :current-page="page"
      @current-change="handleCurrentChange"
      @size-change="handleSizeChange"
      :page-size="limit"
      :page-sizes="[3, 5, 10]"
      layout="prev, pager, next, jumper,->, total, sizes"
      :total="total"
    >
    </el-pagination>
    <!-- //对话框
    :visible.sync控制对话框显示
    Form 组件提供了表单验证的功能，只需要通过 rules 属性传入约定的验证规则，并将 
    Form-Item 的 prop 属性设置为需校验的字段名即可。校验规则参见 async-validator

     -->
    <el-dialog :title="tmForm.id?'修改品牌':'添加品牌'" :visible.sync="dialogFormVisible">
      <!-- form表单 :model属性，这个属性的作用是,把表单的数据收集到那个对象的身上 ，将来表单验证，也需要这个属性-->
      <el-form style="width: 80%" :model="tmForm" :rules="rules" ref='ruleForm'>
        <el-form-item label="品牌名称" label-width="100px" prop="tmName">
          <el-input autocomplete="off" v-model="tmForm.tmName"></el-input>
        </el-form-item>
        <el-form-item label="品牌logo" label-width="100px" prop="logoUrl">
          <!--这里收集数据：不能使用v-model，因为不是表单元素
            action:设置图片上传的地址
            :on-success:可以检测到图片上传成功，当图片上传成功，会执行一次
            :before-upload：可以在上传图片之前，会执行一次

          -->
          <el-upload
            class="avatar-uploader"
            action="/dev-api/admin/product/fileUpload"
            :show-file-list="false"
            :on-success="handleAvatarSuccess"
            :before-upload="beforeAvatarUpload"
          >
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
            <img v-if="tmForm.logoUrl" :src="tmForm.logoUrl" class="avatar" />
            <i v-else class="el-icon-plus avatar-uploader-icon"></i>
          </el-upload>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="addOrUpdateTradeMark"
          >确 定</el-button
        >
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "tradeMark",
  data() {
    return {
      page: 1,
      limit: 3,
      total: 0,
      list: [],
      dialogFormVisible: false,
      tmForm:{
        tmName:'',
        logoUrl:''
      },
      //表单验证规则
      rules:{
        //品牌名称的验证规则
        tmName: [
            { required: true, message: '请输入品牌名称', trigger: 'blur' },
            { min: 2, max: 10, message: '长度在 2 到 10 个字符', trigger: 'change' }
          ],
          //品牌logo
        logoUrl: [
          { required: true, message: '请选择品牌logo', trigger: 'change' }
        ],
      }
    };
  },
  mounted() {
    //获取列表数据方法
    this.getPageList();
  },
  methods: {
    async getPageList() {
      //解构参数
      const { page, limit } = this;
      let result = await this.$API.trademark.reqTradeMarkList(page, limit);
      if (result.code == 200) {
        this.total = result.data.total;
        this.list = result.data.records;
      }
    },
    handleCurrentChange(pager) {
      this.page = pager;
      this.getPageList();
    },
    handleSizeChange(limits) {
      this.limit = limits;
      this.getPageList();
    },
    showDialog() {
      //显示对话框
      this.dialogFormVisible = true;
      //清楚数据
      this.tmForm={tmName:'',logoUrl:''}
    },
    //修改品牌
    updateTradeMark(row) {
      //row, 当前品牌信息

      this.dialogFormVisible = true;
      this.tmForm={...row}
    },
     handleAvatarSuccess(res, file) {
      //res：上传成功之后返回前端数据
      //file:上传成功之后服务器返回前端数据
      //收集品牌图片数据，因为将来需要带给服务器
        this.tmForm.logoUrl = res.data;
      },
      //上传之前
      beforeAvatarUpload(file) {
        const isJPG = file.type === 'image/jpeg';
        const isLt2M = file.size / 1024 / 1024 < 2;

        if (!isJPG) {
          this.$message.error('上传头像图片只能是 JPG 格式!');
        }
        if (!isLt2M) {
          this.$message.error('上传头像图片大小不能超过 2MB!');
        }
        return isJPG && isLt2M;
      },
      //添加按钮（添加品牌|修改品牌）
    addOrUpdateTradeMark() {
      //当全部验证字段通过，再去书写业务逻辑
      this.$refs.ruleForm.validate(async (success) => {
        //如果全部字段符合条件
        if (success) {
          this.dialogFormVisible = false;
          //发请求（添加品牌|修改品牌）
          let result = await this.$API.trademark.reqAddOrUpdateTradeMark(
            this.tmForm
          );
          if (result.code == 200) {
            //弹出信息:添加品牌成功、修改品牌成功
            this.$message({
              type: "success",
              message: this.tmForm.id ? "修改品牌成功" : "添加品牌成功",
            });
            //添加或者修改品牌成功以后，需要再次获取品牌列表进行展示
            //如果添加品牌： 停留在第一页，修改品牌应该留在当前页面
            this.getPageList(this.tmForm.id ? this.page : 1);
          }
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    deleteTradeMark(row){
       this.$confirm(`确定删除${row.tmName}?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(async() => {
          //当用户点击确认按钮
          let result=await this.$API.trademark.reqDeleteTradeMark(row.id)
          if(result.code==200){
            this.$message({
              type: 'success',
              message: '删除成功!'
            });
            this.getPageList()
          }
        }).catch(() => {
          //点击取消
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
    }
  },
};
</script>

<style>
.avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }
  .avatar {
    width: 178px;
    height: 178px;
    display: block;
  }
</style>
<template>
  <div>
    <el-form ref="ruleForm" :model="formData" label-width="80px" :rules="rules">
      <el-form-item label="姓名" prop="name">
        <el-input v-model="formData.name" size="small"></el-input>
      </el-form-item>
      <el-form-item label="性别" prop="sex">
        <el-input v-model="formData.sex" size="small"></el-input>
      </el-form-item>
      <el-form-item label="出生日期" prop="date">
        <el-date-picker
          v-model="formData.date"
          type="date"
      placeholder="选择日期"
      format="yyyy 年 MM 月 dd 日"
        >
        </el-date-picker>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')"
          >立即创建</el-button
        >
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
    </el-form>
    <div>
      <div class="add">
        <el-button type="primary" @click="handleAdd" size="small"
          >新增</el-button
        >
      </div>
      <el-table :data="tableData" style="width: 100%">
        <el-table-column label="序号" type="index" width="50"></el-table-column>
        <el-table-column prop="identity" label="身份">
        
        </el-table-column>
        <el-table-column prop="time" label="发生时间">
         
        </el-table-column>
        <el-table-column prop="category" label="类别">
         
        </el-table-column>
        <el-table-column prop="content" label="具体内容">
         
        </el-table-column>
        <el-table-column fixed="right" label="操作" width="120">
          <template slot-scope="scope">
            <el-button type="text" @click="handleEdit(scope.row)" size="small"
              >编辑</el-button
            >
            <el-button type="text" @click="handleCancel(scope.row.id)" size="small"
              >删除</el-button
            >
          </template>
        </el-table-column>
      </el-table>
    </div>
    <el-dialog
      title="行为记录"
      :visible.sync="dialogVisible"
      width="100%"
    >
    <el-form ref="actionForm" :model="actionFormData" label-width="80px" :rules="actionRules">
      <el-form-item label="身份" prop="identity">
        <el-input v-model="actionFormData.identity" size="small"></el-input>
      </el-form-item>
      <el-form-item label="类别" prop="category">
        <el-input  v-model="actionFormData.category" size="small"></el-input>
      </el-form-item>
      <el-form-item label="具体内容" prop="content">
        <el-input   type="textarea" v-model="actionFormData.content" size="small"></el-input>
      </el-form-item>
      <el-form-item label="发生时间" prop="time">
        <el-date-picker
        v-model="actionFormData.time"
        type="date"
      placeholder="选择日期"
      format="yyyy 年 MM 月 dd 日"
      value-format="yyyy-MM-dd">
    </el-date-picker>
      </el-form-item>
    </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="actionFormSubmit('actionForm')"
          >确 定</el-button
        >
      </span>
    </el-dialog>
  </div>
</template>
<script>
export default {
  data() {
    return {
        dialogVisible:false,
      tableData: [],
      formData: {
        name: "",
        sex: "",
        date: "",
        action: "",
      },
      actionFormData:{
        identity:'',
        category:'',
        content:'',
        time:'',
      },
      rules: {
        date: [
          {
            required: true,
            message: "请选择日期",
            trigger: "change",
          },
        ],

        name: [{ required: true, message: "请填写姓名", trigger: "blur" }],
        sex: [{ required: true, message: "请填写性别", trigger: "blur" }],
      },
      actionRules: {
        time: [
          {
            required: true,
            message: "请选择日期",
            trigger: "change",
          },
        ],

        identity: [{ required: true, message: "请填写姓名", trigger: "blur" }],
        category: [{ required: true, message: "请填写类别", trigger: "blur" }],
        content: [{ required: true, message: "请填写具体内容", trigger: "blur" }],
      },
    };
  },
  methods: {
    //新增数据
    handleAdd() {
      this.dialogVisible=true
    },
    //确认新增数据
    actionFormSubmit(formName){
        this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$message.success("新增成功");
          this.dialogVisible=false
          this.actionFormData.id=new Date().getTime()
          this.tableData.push(this.actionFormData)
          this.actionFormData={
            identity:'',
            category:'',
            content:'',
            time:'',}
        } else {
          console.log("error submit!!");
          return false;
        }
      });
        
    },
    handleEdit(row){
        this.dialogVisible=true
        this.actionFormData={...row}
    },
    //删除数据
    handleCancel(id){
        this.$confirm('是否删除数据？', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
        }).then(() => {
  // 点击确定按钮后要执行的操作
  this.tableData.forEach((ele,index,arr)=>{
            if(ele.id==id) {
                arr.splice(index,1)
            }
        })
}).catch(() => {
  // 点击取消按钮后要执行的操作
});
       
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$message.success("新增成功");
          this.$router.push("/");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
  },
};
</script>
<style scoped>
.add {
  display: flex;
  justify-content: flex-end;
}
</style>
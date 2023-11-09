<template>
  <div class="hello">
    <div class="search">
      <el-input placeholder="请输入学生" v-model="formData.name" size="small"></el-input>
      <el-date-picker
      v-model="formData.timeRange"
      type="datetimerange"
      size="small"
      range-separator="至"
      start-placeholder="开始日期"
      end-placeholder="结束日期">
    </el-date-picker>
      <el-input placeholder="请输入类别" v-model="formData.category" size="small"></el-input>
    </div>
    <div class="add">
      <el-button type="primary" @click="handleAdd" size="small">新增</el-button>
    </div>
    <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column
      label="序号"
      type="index"
      width="50">
    </el-table-column>
      <el-table-column
        prop="name"
        label="姓名">
      </el-table-column>
      <el-table-column
        prop="sex"
        label="性别">
      </el-table-column>
      <el-table-column
        prop="date"
        label="出生日期">
      </el-table-column>
      <el-table-column
      fixed="right"
      label="操作"
      width="120">
      <template slot-scope="scope" >
        <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
        <el-button type="text" @click="handleEdit(scope.row)" size="small">编辑</el-button>
        <el-button type="text" @click="handleCancel(scope.row.id)" size="small">删除</el-button>
      </template>
    </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      formData:{
        name:'',
        timeRange:'',
        category:'',
      },
      tableData:  [{
        id:1,
            date: '2016-05-02',
            name: '一',
            sex: '男'
          }, {
            id:2,
            date: '2016-05-04',
            name: '二',
            sex: '女'
          }, {
            id:3,
            date: '2016-05-01',
            name: '三',
            sex: '男'
          }, {
            id:4,
            date: '2016-05-03',
            name: '四',
            sex: '女'
          }]
    }
  },
  created(){
      console.log('99916',navigator.userAgent)
    },
  methods:{
    handleAdd(){
      this.$router.push({
        path:'/detail',
        query:{
          type:'add'
        }
      })
    },
    handleClick(row){
      this.$router.push({
        path:'/detail',
        query:{
          type:'view'
        }
      })
    },
    handleEdit(row){
      this.$router.push({
        path:'/detail',
        query:{
          type:'view'
        }
      })
    },
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
    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.search {
  display: flex;
}
.add {
  display: flex;
  justify-content: flex-end;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

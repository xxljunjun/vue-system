<template>
  <div class="tailMaidImport">
    <div class="condition">
      <div class="left_box">
        <span class="title_1">日期：</span>
        <el-date-picker
          v-model="myDate"
          type="daterange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          style="width: 300px"
          @change="myDateChange"
          value-format="yyyy-MM-dd"
        >
        </el-date-picker>
        <span class="title_2">基金名称/代码：</span>
        <el-input
          v-model="fundCode"
          placeholder="请输入基金名称/代码"
          style="width: 300px"
        ></el-input>
         <el-button class="search_btn">查询</el-button>
      </div>
      <div class="right_box">
        <importData/>
      </div>
    </div>
    <div class="table_box">
      <el-table v-loading="isLoading" ref="elementTable" :data="tableData" :height="boxHeight" border style="width: 100%">
        <el-table-column prop="date" label="基金名称" width="180">
        </el-table-column>
        <el-table-column prop="name" label="基金代码" width="180">
        </el-table-column>
        <el-table-column prop="address" label="尾佣"> </el-table-column>
        <el-table-column prop="address" label="日期"> </el-table-column>
      </el-table>
    </div>
    <div class="page">
      <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="[20,50,100,200]"
      :page-size="pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="totalNum">
    </el-pagination>
    </div>
  </div>
</template>

<script>
import importData from "@/components/humanResources/importData.vue"
export default {
  name:"tailMaidImport",
   components:{
    importData
  }, 
  data() {
    return {
      boxHeight:0,
      isLoading:false,
      pageSize:20,
      totalNum:100,
      currentPage:1,
      myDate: "",
      fundCode: "",
      tableData: [
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-02",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-04",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-01",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-08",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-06",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-07",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
      ],
    };
  },
  updated(){
    let Height = window.innerHeight || 869
    this.boxHeight = Height-210
  },
  mounted(){
    
  },
  methods:{
    myDateChange(val){
      console.log(">>>",val)
    },
     handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
      },
      handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
      }

  }
};
</script>

<style lang="scss" scoped>
.tailMaidImport {
  color: #000;
  padding: 20px;
  .condition {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
    .title_1 {
      width: 50px;
    }
    .title_2 {
      width: 120px;
      margin-left: 20px;
    }
    .left_box {
      display: flex;
      align-items: center;
      .search_btn{
        margin-left: 20px;
      }
    }
    .right_box {
    }
  }
  .page{
    margin-top: 20px;
    display: flex;
    align-items: center;
    justify-content: flex-end;
  }
}
</style>

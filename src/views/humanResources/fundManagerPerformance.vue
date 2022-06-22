<template>
  <div class="fundManagerPerformance">
    <div class="condition">
      <div class="left_box">
        <span class="title_1">统计日期：</span>
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
        <span class="title_3">基金经理：</span>
        <el-input
          v-model="fundManager"
          placeholder="请输入基金经理"
          style="width: 300px"
        ></el-input>
         <el-button class="search_btn">查询</el-button>
      </div>
      <div class="right_box">
        <exportData  tableName="基金经理业绩考核"/>
      </div>
    </div>
    <div class="table_box">
      <el-table id="out-table" v-loading="isLoading" ref="elementTable" :data="tableData" :height="boxHeight" border style="width: 100%">
        <el-table-column prop="date" label="基金代码" width="180">
        </el-table-column>
        <el-table-column prop="name" label="基金名称" width="180">
        </el-table-column>
         <el-table-column prop="name" label="基金经理" width="180">
        </el-table-column>
        <el-table-column prop="address" label="管理费"> </el-table-column>
        <el-table-column prop="address" label="尾佣"> </el-table-column>
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
import exportData from "@/components/humanResources/exportData.vue"
export default {
  name:"fundManagerPerformance",
  components: {
    exportData
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
      fundManager:"",
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
.fundManagerPerformance {
  color: #000;
  padding: 20px;
  .condition {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
    .title_1 {
      width: 80px;
    }
    .title_2 {
      width: 120px;
      margin-left: 20px;
    }
    .title_3 {
      width: 80px;
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

<template>
  <div class="fundManagerWeights">
    <div class="condition">
      <div class="left_box">
        <span class="title_1">基金名称/代码：</span>
        <el-input
          v-model="fundCode"
          placeholder="请输入基金名称/代码"
          style="width: 300px"
        ></el-input>
        <span class="title_2">基金经理：</span>
        <el-input
          v-model="fundManager"
          placeholder="请输入基金经理"
          style="width: 300px"
        ></el-input>
        <el-button class="search_btn">查询</el-button>
      </div>
      <div class="right_box">
        <el-button class="add_btn" @click="addData">新增</el-button>
        <el-button class="edit_btn" @click="editData">编辑</el-button>
       <exportData tableName="基金经理权重管理"/>
      </div>
    </div>
    <div class="table_box">
      <el-table
        v-loading="isLoading"
        ref="elementTable"
        :data="tableData"
        :height="boxHeight"
        border
        style="width: 100%"
        id="out-table"
      >
        <el-table-column prop="date" label="基金代码" width="180">
        </el-table-column>
        <el-table-column prop="name" label="基金名称" width="180">
        </el-table-column>
        <el-table-column prop="address" label="开始时间"> </el-table-column>
        <el-table-column prop="address" label="结束时间"> </el-table-column>
        <el-table-column prop="address" label="基金经理"> </el-table-column>
      </el-table>
    </div>
    <div class="page">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-sizes="[20, 50, 100, 200]"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="totalNum"
      >
      </el-pagination>
    </div>
    <!-- 弹窗 -->
    <addFundManagerWeight
      :addDialogStatus.sync="addDialogStatus"
      :isEdit="isEdit"
      :backData="backData"
      @ok="submit"
    />
  </div>
</template>

<script>
import addFundManagerWeight from "@/components/humanResources/addFundManagerWeight.vue";
import exportData from "@/components/humanResources/exportData.vue"
export default {
  name: "fundManagerWeights",
  components: {
    addFundManagerWeight,
    exportData
  },
  data() {
    return {
      isEdit: false,
      backData: {},
      addDialogStatus: false,
      boxHeight: 0,
      isLoading: false,
      pageSize: 20,
      totalNum: 100,
      currentPage: 1,
      fundCode: "",
      fundManager: "",
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
  updated() {
    let Height = window.innerHeight || 869;
    this.boxHeight = Height - 210;
  },
  mounted() {},
  methods: {
    submit(data) {
      console.log("编辑或者新增提交的数据", this.isEdit, data);
    },
    editData() {
      this.isEdit = true;
      this.backData = {
        fundCode: "111",
        fundName: "222",
        startTime: "2020-10-10",
        endTime: "2021-11-12",
        fundWeightArr: [
          { fundManager: "小米", weights: "80%" },
          { fundManager: "小蓝", weights: "20%" },
        ],
      };
      this.addDialogStatus = true;
    },
    addData() {
      this.isEdit = false;
      this.backData = {};
      this.addDialogStatus = true;
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    },
  },
};
</script>

<style lang="scss" scoped>
.fundManagerWeights {
  color: #000;
  padding: 20px;
  .condition {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
    .title_1 {
      width: 120px;
    }
    .title_2 {
      width: 80px;
      margin-left: 20px;
    }
    .left_box {
      display: flex;
      align-items: center;
      .search_btn {
        margin-left: 20px;
      }
    }
    .right_box {
      .add_btn {
        margin-right: 20px;
      }
      .edit_btn {
        margin-right: 20px;
      }
    }
  }
  .page {
    margin-top: 20px;
    display: flex;
    align-items: center;
    justify-content: flex-end;
  }
}
</style>

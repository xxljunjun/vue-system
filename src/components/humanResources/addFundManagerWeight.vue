<template>
  <el-dialog
    :title="isEdit ? '编辑基金经理考核权重' : '新增基金经理考核权重'"
    :visible.sync="status"
    width="30%"
    :before-close="handleClose"
    :close-on-click-modal="true"
    :show-close="true"
  >
    <div class="dialog_box">
      <div class="block">
        <span class="red">*</span>
        <span class="title">基金代码：</span>
        <el-input
          v-model="fundCode"
          placeholder="请输入基金代码"
          style="width: 400px"
        ></el-input>
      </div>
      <div class="block">
        <span class="red">*</span>
        <span class="title">基金名称：</span>
        <el-input
          v-model="fundName"
          placeholder="请输入基金名称"
          style="width: 400px"
        ></el-input>
      </div>
      <div class="block">
        <span class="red">*</span>
        <span class="title">开始时间：</span>
        <el-date-picker
          v-model="startTime"
          type="date"
          placeholder="选择开始时间"
          style="width: 400px"
        >
        </el-date-picker>
      </div>
      <div class="block">
        <span class="red">*</span>
        <span class="title">结束时间：</span>
        <el-date-picker
          v-model="endTime"
          type="date"
          placeholder="选择结束时间"
          style="width: 400px"
        >
        </el-date-picker>
      </div>
      <div class="block weights_box">
        <span class="red">*</span>
        <span class="title">基金经理/权重：</span>
        <div class="setting_weights">
          <ul class="head">
            <li>基金经理</li>
            <li>权重</li>
            <li>操作</li>
          </ul>
          <ul class="add_box">
            <li>
              <el-input
                v-model="fundManager"
                placeholder=""
                style="width: 100px"
                size="small"
              ></el-input>
            </li>
            <li>
              <el-input
                v-model="weights"
                placeholder=""
                style="width: 100px"
                size="small"
              ></el-input>
            </li>
            <li class="add_btn" @click="addWeigghts">添加</li>
          </ul>
          <ul class="body" v-for="item in fundWeightArr" :key="item.id">
            <li>{{ item.fundManager }}</li>
            <li>{{ item.weights }}</li>
            <li class="del_btn" @click="delWeights(item)">删除</li>
          </ul>
          <ul class="bottom">
            <li>汇总</li>
            <li>100%</li>
            <li></li>
          </ul>
        </div>
      </div>
    </div>
    <span slot="footer" class="dialog-footer">
      <el-button @click="cancle">取 消</el-button>
      <el-button type="primary" @click="ok">确 定</el-button>
    </span>
  </el-dialog>
</template>

<script>
import { nanoid } from "nanoid";
export default {
  data() {
    return {
      fundCode: "",
      fundName: "",
      startTime: "",
      endTime: "",
      fundWeightArr: [],
      fundManager: "",
      weights: "",
    };
  },
  watch: {
    backData: {
      handler(newVal) {
        console.log("回填数据", newVal);
        if (Object.keys(newVal).length !== 0) {
          this.fundCode = newVal.fundCode;
          this.fundName = newVal.fundName;
          this.startTime = newVal.startTime;
          this.endTime = newVal.endTime;
          this.fundWeightArr = JSON.parse(JSON.stringify(newVal.fundWeightArr));
          this.fundWeightArr.forEach((val) => {
            val.id = nanoid();
          });
        }
      },
      deep: true,
      // immediate:true
    },
  },
  props: {
    addDialogStatus: {
      type: Boolean,
      default: false,
    },
    isEdit: {
      type: Boolean,
      default: false,
    },
    backData: {
      type: Object,
      default: () => {
        return {};
      },
    },
  },
  computed: {
    status: {
      get() {
        return this.addDialogStatus;
      },
      set(val) {
        this.$emit("update:addDialogStatus", val);
      },
    },
  },
  methods: {
    addWeigghts() {
      console.log(">>>", this.fundManager);
      console.log(">>>", this.weights);
      if (this.fundManager == "") {
        this.$message({
          message: "请填写基金经理名称！",
          type: "warning",
        });
        return;
      }
      if (this.weights == "") {
        this.$message({
          message: "请填写权重！",
          type: "warning",
        });
        return;
      }
      let obj = {
        id: nanoid(),
        fundManager: this.fundManager,
        weights: this.weights,
      };
      console.log(obj);
      this.fundWeightArr.push(obj);
      //添加成功后清空输入框
      this.fundManager = "";
      this.weights = "";
    },
    delWeights(item) {
      console.log(">>>", item);
      let id = item.id;
      this.fundWeightArr = this.fundWeightArr.filter((val) => {
        return val.id != id;
      });
    },
    handleClose() {
      this.$emit("update:addDialogStatus", false);
    },
    cancle() {
      this.$emit("update:addDialogStatus", false);
    },
    ok() {
      let obj = {
        fundCode: this.fundCode,
        fundName: this.fundName,
        startTime: this.startTime,
        endTime: this.endTime,
        fundWeightArr: this.fundWeightArr,
      };
      this.$emit("ok", obj);
    },
  },
};
</script>

<style lang="scss" scoped>
.dialog_box {
  display: flex;
  flex-direction: column;
  .block {
    margin-bottom: 15px;
    .title {
      text-align: justify;
      text-align-last: justify;
      display: inline-block;
      width: 110px;
    }
    .red {
    }
  }
  .weights_box {
    display: flex;
    flex-wrap: wrap;
    margin-bottom: 0;
  }
}
.setting_weights {
  display: flex;
  flex-direction: column;
  border-left: 1px solid #ccc;
  border-top: 1px solid #ccc;
  ul {
    display: flex;
    flex-direction: row;
    margin: 0;
    padding: 0;
    li {
      list-style: none;
      width: 133px;
      height: 40px;
      display: flex;
      justify-content: center;
      align-items: center;
      border-right: 1px solid #ccc;
      border-bottom: 1px solid #ccc;
    }
  }
  .head {
    li {
      font-weight: 600;
    }
  }
  .add_box {
    .add_btn {
      color: blue;
      cursor: pointer;
    }
  }
  .body {
    .del_btn {
      color: blue;
      cursor: pointer;
    }
  }
  .bottom {
    li {
      font-weight: 600;
    }
  }
}
</style>

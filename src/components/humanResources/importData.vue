<template>
  <div class="importData">
    <el-upload
      class="upload-demo"
      action="https://jsonplaceholder.typicode.com/posts/"
      :multiple="false"
      :show-file-list="false"
      :on-success="handlerSuccess"
      :on-error="handlerError"
      :before-upload="beforeUpload"
    >
      <el-button type="primary">导入</el-button>
    </el-upload>
  </div>
</template>

<script>
import excel from "@/utils/excel.js"
export default {
  name: "importData",
  data() {
    return {
      fileExt:"",
        file:'',
        fileExtName:'',
        uploadFile:'',
        fileExt:'',

    };
  },
  methods: {
    
  },
  computed: {},
  methods: {
    handlerError(err, file, fileList){
      console.log("上传失败file",file)
    },
    handlerSuccess(response, file, fileList){
      console.log("上传成功file",file)
    },
    /**
     * @function 上传文件前
     */
    beforeUpload(file) {
      console.log("需要上传的文件", file);
      const fileExt = file.name
        .split(".")
        .pop()
        .toLocaleLowerCase();
      console.log("file.name>>>", file.name);
      this.fileExtName = file.name;
      this.uploadFile = file;
      if (fileExt === "xlsx" || fileExt === "xls") {
        this.fileExt = fileExt;
        this.readFile(file);
        this.file = file;
      } else {
        this.$message({
          message: `文件:${file.name}不是EXCEL文件,请选择后缀为.xlsx或者.xls的EXCEL文件。`,
          type: "warning",
        });
      }
      return false;
    },
     /**
     * @function 解析文件
     */
    readFile(file) {
      const reader = new FileReader();
      reader.readAsArrayBuffer(file);
      reader.onloadstart = e => {
        this.uploadLoading = true;
        this.tableLoading = true;
        this.showProgress = true;
      };
      reader.onprogress = e => {
        this.progressPercent = Math.round((e.loaded / e.total) * 100);
      };
      reader.onerror = e => {
        this.$hMessage.error("文件读取出错");
      };
      reader.onload = e => {
        console.log(">>",e)
        const data = e.target.result;
        const { header, results } = excel.read(data, "array");
        const tableTitle = header.map(item => {
          return { title: item, key: item };
        });
        console.log("解析的数据>>>>>>>", results);
        console.log("tableTitle>>>>>>>", tableTitle);
        importData().then(res=>{
          console.log("导入数据成功了")
        })
      };
    },
    
  },
};
</script>

<style lang="scss" scoped>
.importData {
}
</style>

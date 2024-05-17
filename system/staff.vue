
<template>
  <div>
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item label="姓名">
        <el-input
          v-model="formInline.user"
          placeholder="请输入员工姓名"
        ></el-input>
      </el-form-item>

      <el-form-item label="性别">
        <el-select v-model="formInline.gender" placeholder="请选择">
          <el-option label="男" value="male"></el-option>
          <el-option label="女" value="female"></el-option>
        </el-select>
      </el-form-item>

      <el-form-item label="入职时间">
        <el-date-picker
          v-model="formInline.time"
          type="daterange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
        >
        </el-date-picker>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>

    <el-form :inline="true" class="demo-form-inline">
      <el-form-item label="   ">
        <el-button type="primary" @click="addVisible = true"
          >新增员工</el-button
        >
        <el-dialog title="新增员工" :visible.sync="addVisible" width="30%">
          <el-form :model="add_form" label-position="right" label-width="80px">
            <el-form-item label="用户名">
              <el-input
                v-model="add_form.username"
                autocomplete="off"
              ></el-input>
            </el-form-item>
            <el-form-item label="上传图片">
              <el-upload
                class="avatar-uploader"
                action="http://localhost:8080/upload"
                :show-file-list="false"
                :on-success="handleAddSuccess"
                :before-upload="beforeAddUpload"
              >
                <img
                  v-if="add_form.image"
                  :src="add_form.image"
                  class="avatar"
                />
                <i v-else class="el-icon-plus avatar-uploader-icon"></i>
              </el-upload>
            </el-form-item>
            <el-form-item label="员工姓名">
              <el-input v-model="add_form.name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="性别">
              <el-select v-model="add_form.gender" placeholder="请选择">
                <el-option label="男" value="1"></el-option>
                <el-option label="女" value="2"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="职位">
              <el-select v-model="add_form.job" placeholder="请选择">
                <el-option label="班主任" value="1"></el-option>
                <el-option label="讲师" value="2"></el-option>
                <el-option label="学工主管" value="3"></el-option>
                <el-option label="教研主管" value="4"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="入职日期" label-width="70px">
              <el-date-picker
                v-model="add_form.entryDate"
                type="date"
                placeholder="选择日期"
              >
              </el-date-picker>
            </el-form-item>
            <el-form-item label="归属部门">
              <el-select v-model="add_form.deptId" placeholder="请选择">
                <el-option label="学工部" value="1"></el-option>
                <el-option label="后勤部" value="2"></el-option>
                <el-option label="就业部" value="3"></el-option>
              </el-select>
            </el-form-item>
          </el-form>

          <span slot="footer" class="dialog-footer">
            <el-button @click="addVisible = false">取 消</el-button>
            <el-button
              v-on:click="add()"
              type="primary"
              @click="addVisible = false"
              >确 定</el-button
            >
          </span>
        </el-dialog>
      </el-form-item>
      <el-form-item>
        <el-button type="danger" @click="del">批量删除</el-button>
      </el-form-item>
    </el-form>

    <el-table
      ref="multipleTable"
      :data="tableData"
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" width="55"> </el-table-column>
      <el-table-column prop="name" label="姓名" width="120"> </el-table-column>
      <el-table-column prop="image" label="图像" width="120">
        <template slot-scope="scope">
          <img :src="scope.row.image" width="100px" height="70px" />
        </template>
      </el-table-column>
      <el-table-column prop="username" label="用户命" width="120">
      </el-table-column>
      <el-table-column prop="gender" label="性别" width="120">
      </el-table-column>
      <el-table-column prop="job" label="职位" width="120"> </el-table-column>
      <el-table-column prop="createTime" label="入职日期" width="180">
      </el-table-column>
      <el-table-column prop="updateTime" label="最后操作日期" width="180">
      </el-table-column>

      <el-table-column label="操作" width="500">
        <template slot-scope="scope">
          <el-button
            type="primary"
            size="small"
            @click="changeVisible = true"
            v-on:click="get_change(scope.row.id)"
            >编辑</el-button
          >
          <el-dialog title="编辑员工" :visible.sync="changeVisible" width="30%">
            <el-form
              :model="change_form"
              label-position="right"
              label-width="80px"
            >
              <el-form-item label="用户名">
                <el-input
                  v-model="change_form.username"
                  autocomplete="off"
                ></el-input>
              </el-form-item>
              <el-form-item label="上传图片">
                <el-upload
                  class="avatar-uploader"
                  action="http://localhost:8080/upload"
                  :show-file-list="false"
                  :on-success="handleChangeSuccess"
                  :before-upload="beforeChangeUpload"
                >
                  <img
                    v-if="change_form.image"
                    :src="change_form.image"
                    class="avatar"
                  />
                  <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                </el-upload>
              </el-form-item>
              <el-form-item label="员工姓名">
                <el-input
                  v-model="change_form.name"
                  autocomplete="off"
                ></el-input>
              </el-form-item>
              <el-form-item label="性别">
                <el-select v-model="change_form.gender" placeholder="请选择">
                  <el-option label="男" value="1"></el-option>
                  <el-option label="女" value="2"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="职位">
                <el-select v-model="change_form.job" placeholder="请选择">
                  <el-option label="班主任" value="1"></el-option>
                  <el-option label="讲师" value="2"></el-option>
                  <el-option label="学工主管" value="3"></el-option>
                  <el-option label="教研主管" value="4"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="入职日期">
                <el-date-picker
                  v-model="change_form.entryDate"
                  type="date"
                  placeholder="选择日期"
                >
                </el-date-picker>
              </el-form-item>
              <el-form-item label="归属部门">
                <el-select v-model="change_form.deptId" placeholder="请选择">
                  <el-option label="学工部" value="1"></el-option>
                  <el-option label="后勤部" value="2"></el-option>
                  <el-option label="就业部" value="3"></el-option>
                </el-select>
              </el-form-item>
            </el-form>

            <span slot="footer" class="dialog-footer">
              <el-button @click="changeVisible = false">取 消</el-button>
              <el-button
                v-on:click="change()"
                type="primary"
                @click="changeVisible = false"
                >确 定</el-button
              >
            </span>
          </el-dialog>
        </template>
      </el-table-column>
    </el-table>

    <br />

    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page.sync="currentPage"
      :page-sizes="pageSizes"
      :page-size="pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
    >
    </el-pagination>
  </div>
</template>
  
  
  
  
  
    <script>
export default {
  data() {
    return {
      formInline: {
        user: "",
        gender: "",
        time: {
          start: "",
          end: "",
        },
      },
      delVisible: false,
      changeVisible: false,
      addVisible: false,
      formLabelWidth: "120px",
      cid: "",
      did: "",
      new_form: {
        name: "",
      },
      add_form: {
        username: "",
        name: "",
        image: "",
        gender: "",
        job: "",
        entryDate: "",
        deptId: "",
      },
      change_form: {
        username: "",
        name: "",
        image: "",
        gender: "",
        job: "",
        entryDate: "",
        deptId: "",
      },
      tableData: [],
      currentPage: 1,
      //可选的每条显示几条数据
      pageSizes: [5, 10, 15, 20],
      //每页显示的条数
      pageSize: 5,
      //总条数
      total: 0,
      multipleSelection: [],
      ids: [],
    };
  },
  methods: {
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
      this.pageSize = val;
      this.new();
    },
    handleCurrentChange(val) {
      this.currentPage = val;
      this.new();
    },
    onSubmit() {
      this.new();
    },
    new() {
      var _this = this;
      const params = {
        page: this.currentPage,
        pageSize: this.pageSize,
        name: this.formInline.user,
        gender: this.formInline.gender,
        begin: this.formInline.time.start,
        end: this.formInline.time.end,
      };
      this.axios
        .get("http://localhost:8080/emps", { params })
        .then(function (response) {
          _this.tableData = response.data.data.rows;
          _this.total = response.data.data.total;
          console.log(_this.tableData);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
      this.ids = this.multipleSelection.map((item) => item.id);
      console.log(this.ids);
    },

    get_did(id) {
      this.did = id;
      this.delVisible = true;
    },
    change() {
      const changeData = {
        id: this.cid,
        username: this.change_form.username,
        image: this.change_form.image,
        name: this.change_form.name,
        gender: this.change_form.gender,
        job: this.change_form.job,
        entrydate: this.change_form.entryDate,
        deptId: this.change_form.deptId,
      };
      this.axios
        .put("http://localhost:8080/emps", changeData)
        .then((response) => {
          console.log("Resource changed successfully:", response.data);
          this.new();
        })
        .catch(function (error) {
          console.log(error);
        });
    },

    del() {
      const idString = this.ids.join(",");
      const url = `http://localhost:8080/emps/${idString}`;
      this.axios
        .delete(url)
        .then((response) => {
          console.log("Resource deleted successfully:", response.data);
          this.new();
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    add() {
      const addData = {
        username: this.add_form.username,
        name: this.add_form.name,
        image: this.add_form.image,
        gender: this.add_form.gender,
        job: this.add_form.job,
        entrydate: this.add_form.entryDate,
        deptId: this.add_form.deptId,
      };
      this.axios
        .post("http://localhost:8080/emps", addData)
        .then((response) => {
          console.log("Resource added successfully:", response.data);
          this.new();
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    get_change(id) {
      const url = `http://localhost:8080/emps/${id}`;
      this.cid = id;
      this.axios
        .get(url)
        .then((response) => {
          console.log(response.data);
          const list = response.data.data;
          this.change_form.username = list.username;
          this.change_form.image = list.image;
          this.change_form.name = list.name;
          this.change_form.gender = list.gender;
          this.change_form.job = list.job;
          this.change_form.entryDate = list.entrydate;
          this.change_form.deptId = list.deptId;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    handleAddSuccess(res) {
      this.add_form.image = res.data;
    },
    handleChangeSuccess(res) {
      this.change_form.image = res.data;
    },
    beforeAddUpload(file) {
      const isJPG = file.type === "image/jpeg";
      const isLt2M = file.size / 1024 / 1024 < 2;

      if (!isJPG) {
        this.$message.error("上传头像图片只能是 JPG 格式!");
      }
      if (!isLt2M) {
        this.$message.error("上传头像图片大小不能超过 2MB!");
      }
      return isJPG && isLt2M;
    },
    beforeChangeUpload(file) {
      const isJPG = file.type === "image/jpeg";
      const isLt2M = file.size / 1024 / 1024 < 2;

      if (!isJPG) {
        this.$message.error("上传头像图片只能是 JPG 格式!");
      }
      if (!isLt2M) {
        this.$message.error("上传头像图片大小不能超过 2MB!");
      }
      return isJPG && isLt2M;
    },
  },
  mounted() {
    this.new();
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
  border-color: #409eff;
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
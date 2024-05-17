

<template>
  <div>
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item label="学员姓名">
        <el-input
          v-model="formInline.name"
          placeholder="请输入学员姓名"
        ></el-input>
      </el-form-item>
      <el-form-item label="学号">
        <el-input v-model="formInline.num" placeholder="请输入学号"></el-input>
      </el-form-item>
      <el-form-item label="最高学历">
        <el-select v-model="formInline.xueli" placeholder="请选择">
          <el-option
            v-for="item in xueli"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          >
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="所属班级">
        <el-select v-model="formInline.classes" placeholder="请选择">
          <el-option
            v-for="item in classes"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          >
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>

    <br />

    <el-form :inline="true" class="demo-form-inline">
      <el-form-item label="   ">
        <el-button type="primary" @click="addVisible = true"
          >添加学员</el-button
        >

        <el-dialog title="新增学员" :visible.sync="addVisible">
          <el-form :model="add_form">
            <el-form-item label="姓名" :label-width="formLabelWidth">
              <el-input v-model="add_form.name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="学号" :label-width="formLabelWidth">
              <el-input v-model="add_form.num" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item label="性别" :label-width="formLabelWidth">
              <el-select v-model="add_form.gender" placeholder="请选择">
                <el-option label="男" value="1"></el-option>
                <el-option label="女" value="2"></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="手机号" :label-width="formLabelWidth">
              <el-input v-model="add_form.phone" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item label="最高学历" :label-width="formLabelWidth">
              <el-select v-model="add_form.xueli" placeholder="请选择">
                <el-option label="大专" value="1"></el-option>
                <el-option label="本科" value="2"></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="所属班级" :label-width="formLabelWidth">
              <el-select v-model="add_form.classes" placeholder="请选择">
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                >
                </el-option>
              </el-select>
            </el-form-item>
          </el-form>

          <div slot="footer" class="dialog-footer">
            <el-button @click="addVisible = false">取 消</el-button>
            <el-button
              type="primary"
              @click="addVisible = false"
              v-on:click="add()"
              >确 定</el-button
            >
          </div>
        </el-dialog>
      </el-form-item>

      <el-form-item>
        <el-button type="danger" @click="delVisible = true">批量删除</el-button>
        <el-dialog title="删除学员" :visible.sync="delVisible" width="30%">
          <span>您确定要删除吗？</span>
          <span slot="footer" class="dialog-footer">
            <el-button @click="delVisible = false">取 消</el-button>
            <el-button
              v-on:click="del()"
              type="primary"
              @click="delVisible = false"
              >确 定</el-button
            >
          </span>
        </el-dialog>
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

      <el-table-column prop="num" label="学号" width="120"> </el-table-column>

      <el-table-column prop="classes" label="班级" width="120">
      </el-table-column>

      <el-table-column prop="gender" label="性别" width="120">
      </el-table-column>

      <el-table-column prop="phone" label="手机号" width="120">
      </el-table-column>

      <el-table-column prop="xueli" label="最高学历" width="120">
      </el-table-column>

      <el-table-column prop="wrong" label="违纪次数" width="120">
      </el-table-column>

      <el-table-column prop="score" label="违纪扣分" width="120">
      </el-table-column>

      <el-table-column label="操作" width="500">
        <template slot-scope="scope">
          <el-button
            type="text"
            size="small"
            @click="changeVisible = true"
            v-on:click="get_change(scope.row.id)"
            >编辑</el-button
          >
          <el-dialog title="编辑学员" :visible.sync="changeVisible">
            <el-form :model="change_form">
              <el-form-item label="姓名" :label-width="formLabelWidth">
                <el-input
                  v-model="change_form.name"
                  autocomplete="off"
                ></el-input>
              </el-form-item>
              <el-form-item label="学号" :label-width="formLabelWidth">
                <el-input
                  v-model="change_form.num"
                  autocomplete="off"
                ></el-input>
              </el-form-item>

              <el-form-item label="性别" :label-width="formLabelWidth">
                <el-select v-model="change_form.gender" placeholder="请选择">
                  <el-option label="男" value="1"></el-option>
                  <el-option label="女" value="2"></el-option>
                </el-select>
              </el-form-item>

              <el-form-item label="手机号" :label-width="formLabelWidth">
                <el-input
                  v-model="change_form.phone"
                  autocomplete="off"
                ></el-input>
              </el-form-item>

              <el-form-item label="最高学历" :label-width="formLabelWidth">
                <el-select v-model="change_form.xueli" placeholder="请选择">
                  <el-option label="大专" value="1"></el-option>
                  <el-option label="本科" value="2"></el-option>
                </el-select>
              </el-form-item>

              <el-form-item label="所属班级" :label-width="formLabelWidth">
                <el-select v-model="change_form.classes" placeholder="请选择">
                  <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                  >
                  </el-option>
                </el-select>
              </el-form-item>
            </el-form>

            <div slot="footer" class="dialog-footer">
              <el-button @click="changeVisible = false">取 消</el-button>
              <el-button
                type="primary"
                @click="changeVisible = false"
                v-on:click="change()"
                >确 定</el-button
              >
            </div>
          </el-dialog>

          <el-button type="text" size="small" @click="get_wid(scope.row.id)"
            >违纪</el-button
          >
          <el-dialog title="违纪扣分" :visible.sync="wVisible" width="30%">
            <el-input v-model="score" autocomplete="off"></el-input>
            <span slot="footer" class="dialog-footer">
              <el-button @click="wVisible = false">取 消</el-button>
              <el-button
                v-on:click="wrong()"
                type="primary"
                @click="wVisible = false"
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
        name: "",
        num: "",
        classes: "",
        xueli: "",
      },
      classes: [
        {
          value: "2024年第一期01班",
          label: "2024年第一期01班",
        },
        {
          value: "2024年第一期02班",
          label: "2024年第一期02班",
        },
      ],
      value: "",
      xueli: [
        {
          value: "1",
          label: "大专",
        },
        {
          value: "2",
          label: "本科",
        },
        {
          value: "3",
          label: "硕士",
        },
        {
          value: "4",
          label: "博士",
        },
        {
          value: "5",
          label: "初中",
        },
      ],
      tableData: [],
      currentPage: 1,
      //可选的每条显示几条数据
      pageSizes: [5, 10, 15, 20],
      //每页显示的条数
      pageSize: 5,
      //总条数
      total: 0,
      form: {
        name: "",
        num: "",
        phone: "",
      },

      options: [
        {
          value: "1",
          label: "2211101",
        },
        {
          value: "2",
          label: "2211102",
        },
      ],
      addVisible: false,
      formLabelWidth: "100px",
      add_form: {
        name: "",
        num: "",
        gender: "",
        classes: "",
        phone: "",
        xueli: "",
        wrong: 0,
        score: 0,
      },
      change_form: {
        name: "",
        num: "",
        gender: "",
        classes: "",
        phone: "",
        xueli: "",
        wrong: "",
        score: "",
      },
      ids: [],
      delVisible: false,
      changeVisible: false,
      wVisible: false,
      cid: "",
      wid: "",
      score: "",
    };
  },
  methods: {
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
      this.pageSize = val;
      this.new();
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
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
        name: this.formInline.name,
        num: this.formInline.num,
        xueli: this.formInline.xueli,
        classes: this.formInline.classes,
      };
      this.axios
        .get("http://localhost:8080/student", { params })
        .then(function (response) {
          _this.tableData = response.data.data.rows;
          _this.total = response.data.data.total;
          console.log(response);
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
    add() {
      const addData = {
        name: this.add_form.name,
        num: this.add_form.num,
        gender: this.add_form.gender,
        phone: this.add_form.phone,
        xueli: this.add_form.xueli,
        wrong: this.add_form.wrong,
        score: this.add_form.score,
        classes: this.add_form.classes,
      };
      this.axios
        .post("http://localhost:8080/student", addData)
        .then((response) => {
          console.log("Resource added successfully:", response.data);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    del() {
      const idString = this.ids.join(",");
      const url = `http://localhost:8080/student/${idString}`;
      this.axios
        .delete(url)
        .then((response) => {
          console.log("Resource deleted successfully:", response.data);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    get_change(id) {
      const url = `http://localhost:8080/student/${id}`;
      this.cid = id;
      this.axios
        .get(url)
        .then((response) => {
          console.log(response.data);
          const list = response.data.data;
          this.change_form.name = list.name;
          this.change_form.num = list.num;
          this.change_form.gender = list.gender;
          this.change_form.phone = list.phone;
          this.change_form.classes = list.classes;
          this.change_form.xueli = list.xueli;
          this.change_form.wrong = list.wrong;
          this.change_form.score = list.score;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    change() {
      const changeData = {
        id: this.cid,
        name: this.change_form.name,
        num: this.change_form.num,
        gender: this.change_form.gender,
        phone: this.change_form.phone,
        classes: this.change_form.classes,
        xueli: this.change_form.xueli,
      };
      this.axios
        .put("http://localhost:8080/student", changeData)
        .then((response) => {
          console.log("Resource changed successfully:", response.data);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    get_wid(id) {
      this.wid = id;
      this.wVisible = true;
    },
    wrong() {
      this.get_change(this.wid);
      const a = Number(this.change_form.wrong) + 1;
      const b = Number(this.score) + Number(this.change_form.score);
      const changeData = {
        id: this.wid,
        wrong: a,
        score: b,
      };
      this.axios
        .put("http://localhost:8080/student", changeData)
        .then((response) => {
          console.log("Resource changed successfully:", response.data);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
  },
  mounted() {
    this.new();
  },
};
</script>
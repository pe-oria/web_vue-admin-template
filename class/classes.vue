

<template>
  <div>
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item label="班级名称">
        <el-input v-model="formInline.classes" placeholder=""
          >班级名称</el-input
        >
      </el-form-item>
      <el-form-item label="结课时间">
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

    <el-button type="primary" @click="addVisible = true">新增班级</el-button>
    <el-dialog title="新增班级" :visible.sync="addVisible">
      <el-form :model="form">
        <el-form-item label="班级名称" :label-width="formLabelWidth">
          <el-input v-model="add_form.classname" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="班级教室" :label-width="formLabelWidth">
          <el-input v-model="add_form.classroom" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="开始时间" :label-width="formLabelWidth">
          <el-date-picker
            v-model="add_form.start"
            type="datetime"
            placeholder="选择日期时间"
          >
          </el-date-picker>
        </el-form-item>

        <el-form-item label="结束时间" :label-width="formLabelWidth">
          <el-date-picker
            v-model="add_form.end"
            type="datetime"
            placeholder="选择日期时间"
          >
          </el-date-picker>
        </el-form-item>

        <el-form-item label="班主任" :label-width="formLabelWidth">
          <el-select v-model="add_form.teacher" placeholder="请选择">
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
        <el-button type="primary" @click="addVisible = false" v-on:click="add()"
          >确 定</el-button
        >
      </div>
    </el-dialog>

    <el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="num" label="序号" width="50" type="index">
      </el-table-column>

      <el-table-column prop="classname" label="班级名称" width="180">
      </el-table-column>

      <el-table-column prop="classroom" label="班级教室"> </el-table-column>

      <el-table-column prop="startdate" label="开课时间" width="180">
      </el-table-column>

      <el-table-column prop="enddate" label="结课时间" width="180">
      </el-table-column>

      <el-table-column prop="teacher" label="班主任"> </el-table-column>

      <el-table-column label="操作" width="500">
        <template slot-scope="scope">
          <el-button
            type="primary"
            size="small"
            @click="changeVisible = true"
            v-on:click="get_change(scope.row.id)"
            >编辑</el-button
          >
          <el-dialog title="修改班级" :visible.sync="changeVisible" width="30%">
            <el-form
              :model="change_form"
              label-position="right"
              label-width="80px"
            >
              <el-form-item label="班级名称">
                <el-input
                  v-model="change_form.classname"
                  autocomplete="off"
                ></el-input>
              </el-form-item>

              <el-form-item label="班级教室">
                <el-input
                  v-model="change_form.classroom"
                  autocomplete="off"
                ></el-input>
              </el-form-item>
              <el-form-item label="开课时间">
                <el-date-picker
                  v-model="change_form.start"
                  type="date"
                  placeholder="选择日期"
                >
                </el-date-picker>
              </el-form-item>
              <el-form-item label="结课时间">
                <el-date-picker
                  v-model="change_form.end"
                  type="date"
                  placeholder="选择日期"
                >
                </el-date-picker>
              </el-form-item>
              <el-form-item label="职位">
                <el-select v-model="change_form.job" placeholder="请选择">
                  <el-option label="赵敏" value="1"></el-option>
                  <el-option label="周芷若" value="2"></el-option>
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

          <el-button type="danger" size="small" @click="get_did(scope.row.id)"
            >删除</el-button
          >
          <el-dialog title="删除部门" :visible.sync="delVisible" width="30%">
            <span>您确定要删除这个部门吗？</span>
            <span slot="footer" class="dialog-footer">
              <el-button @click="delVisible = false">取 消</el-button>
              <el-button
                v-on:click="del(did)"
                type="primary"
                @click="delVisible = false"
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
  mounted() {
    this.new();
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
        classname: this.formInline.classes,
        begin: this.formInline.time.start,
        end: this.formInline.time.end,
      };
      this.axios
        .get("http://localhost:8080/class", { params })
        .then(function (response) {
          _this.tableData = response.data.data.rows;
          _this.total = response.data.data.total;
          console.log(_this.tableData);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    add() {
      const addData = {
        classname: this.add_form.classname,
        classroom: this.add_form.classroom,
        startdate: this.add_form.start,
        enddate: this.add_form.end,
        teacher: this.add_form.teacher,
      };
      this.axios
        .post("http://localhost:8080/class", addData)
        .then((response) => {
          console.log("Resource added successfully:", response.data);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    get_change(id) {
      const url = `http://localhost:8080/class/${id}`;
      this.cid = id;
      this.axios
        .get(url)
        .then((response) => {
          console.log(response.data);
          const list = response.data.data;
          this.change_form.classname = list.classname;
          this.change_form.classroom = list.classroom;
          this.change_form.start = list.startdate;
          this.change_form.end = list.enddate;
          this.change_form.teacher = list.teacher;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    change() {
      const changeData = {
        id: this.cid,
        classname: this.change_form.classname,
        classroom: this.change_form.classroom,
        startdate: this.change_form.start,
        enddate: this.change_form.end,
        teacher: this.change_form.teacher,
      };
      this.axios
        .put("http://localhost:8080/class", changeData)
        .then((response) => {
          console.log("Resource changed successfully:", response.data);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    get_did(id) {
      this.did = id;
      this.delVisible = true;
    },
    del(id) {
      const url = `http://localhost:8080/class/${id}`;
      this.axios
        .delete(url)
        .then((response) => {
          console.log("Resource deleted successfully:", response.data);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
  },
  data() {
    return {
      currentPage: 1,
      //可选的每条显示几条数据
      pageSizes: [5, 10, 15, 20],
      //每页显示的条数
      pageSize: 5,
      //总条数
      total: 0,
      addVisible: false,
      changeVisible: false,
      delVisible: false,
      add_form: {
        classname: "",
        classroom: "",
        start: "",
        end: "",
        teacher: "",
      },
      change_form: {
        classname: "",
        classroom: "",
        start: "",
        end: "",
        teacher: "",
      },
      tableData: [],
      formInline: {
        classes: "",
        time: {
          start: "",
          end: "",
        },
      },
      did: "",
      cid: "",
      form: {
        name: "",
        room: "",
        date1: "",
        date2: "",
        delivery: false,
        type: [],
        resource: "",
        desc: "",
      },
      formLabelWidth: "120px",
      options: [
        {
          value: "1",
          label: "赵敏",
        },
        {
          value: "2",
          label: "周芷若",
        },
      ],

      value: "",
      valuer: [],

      form2: {
        name: "",
        room: "",
        date1: "",
        date2: "",
        delivery: false,
        type: [],
        resource: "",
        desc: "",
      },

      options2: [
        {
          value: "选项1",
          label: "冰淇淋",
        },
        {
          value: "选项2",
          label: "双皮奶",
        },
        {
          value: "选项3",
          label: "蚵仔煎",
        },
        {
          value: "选项4",
          label: "龙须面",
        },
        {
          value: "选项5",
          label: "北京烤鸭",
        },
      ],
    };
  },
};
</script>




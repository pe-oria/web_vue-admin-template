/* eslint-disable */
import axios from 'axios'

<template >
  <div>
    <br />

    <el-form :inline="true">
      <el-form-item label="   ">
        <el-button type="primary" @click="addVisible = true"
          >新增部门</el-button
        >
        <el-dialog title="新增部门" :visible.sync="addVisible">
          <el-form :model="form">
            <el-form-item label="部门名称" :label-width="formLabelWidth">
              <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="addVisible = false">取 消</el-button>
            <el-button
              v-on:click="add"
              type="primary"
              @click="addVisible = false"
              >确 定</el-button
            >
          </div>
        </el-dialog>
      </el-form-item>
    </el-form>

    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="num" label="序号" width="200" type="index">
      </el-table-column>
      <el-table-column prop="name" label="部门名称" width="200">
      </el-table-column>

      <el-table-column prop="updateTime" label="最后操作时间" width="200">
      </el-table-column>

      <el-table-column label="操作" width="500">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="get_cid(scope.row.id)"
            >编辑</el-button
          >
          <el-dialog title="编辑部门" :visible.sync="changeVisible">
            <el-form :model="new_form">
              <el-form-item label="部门名称" :label-width="formLabelWidth">
                <el-input v-model="new_form.name" autocomplete="off"></el-input>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="changeVisible = false">取 消</el-button>
              <el-button
                v-on:click="change(cid)"
                type="primary"
                @click="changeVisible = false"
                >确 定</el-button
              >
            </div>
          </el-dialog>

          <el-button type="text" size="small" @click="get_did(scope.row.id)"
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
  </div>
</template>

  <script>
export default {
  mounted() {
    this.new();
  },

  methods: {
    new() {
      var _this = this;
      this.axios
        .get("http://localhost:8080/depts")
        .then(function (response) {
          console.log(response.data.data);
          console.log(_this.tableData);
          _this.tableData = response.data.data;
          // const list = response.data.data;
          // list.forEach((dep, index) => {
          //   dep.num = index + 1;
          // });
          //_this.tableData = list;
          console.log(_this.tableData);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    add() {
      var _this = this;
      this.axios
        .post("http://localhost:8080/depts", {
          name: _this.form.name,
        })
        .then(function (response) {
          console.log(response);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    change(id) {
      var _this = this;
      const changeData = {
        id: id,
        name: _this.new_form.name,
      };
      this.axios
        .put("http://localhost:8080/depts", changeData)
        .then(function (response) {
          console.log(response);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    del(id) {
      const url = `http://localhost:8080/depts/${id}`;
      this.axios
        .delete(url)
        .then((response) => {
          console.log("Resource deleted successfully:", response.data);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    get_cid(id) {
      this.cid = id;
      this.changeVisible = true;
    },
    get_did(id) {
      this.did = id;
      this.delVisible = true;
    },
  },

  data() {
    return {
      tableData: [],
      addVisible: false,
      delVisible: false,
      changeVisible: false,
      form: {
        name: "",
      },
      new_form: {
        name: "",
      },
      formLabelWidth: "120px",
      cid: "",
      did: "",
    };
  },
};
</script>

<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">编号</label>
        <el-input
          v-model="query.foodNo"
          clearable
          placeholder="编号"
          style="width: 185px;"
          class="filter-item"
          @keyup.enter.native="crud.toQuery"
        />
        <label class="el-form-item-label">名字</label>
        <el-input
          v-model="query.foodName"
          clearable
          placeholder="名字"
          style="width: 185px;"
          class="filter-item"
          @keyup.enter.native="crud.toQuery"
        />
        <rrOperation :crud="crud" />
      </div>
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog
        :close-on-click-modal="false"
        :before-close="crud.cancelCU"
        :visible.sync="crud.status.cu > 0"
        :title="crud.status.title"
        width="500px"
      >
        <el-form
          ref="form"
          :model="form"
          :rules="rules"
          size="small"
          label-width="80px"
        >
          <el-form-item label="id" prop="id">
            <el-input v-model="form.id" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="编号">
            <el-input v-model="form.foodNo" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="名字">
            <el-input v-model="form.foodName" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="介绍" prop="foodInstruction">
            <el-input v-model="form.foodInstruction" style="width: 370px;" />
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button type="text" @click="crud.cancelCU">取消</el-button>
          <el-button
            :loading="crud.status.cu === 2"
            type="primary"
            @click="crud.submitCU"
            >确认</el-button
          >
        </div>
      </el-dialog>
      <!--表格渲染-->
      <el-table
        ref="table"
        v-loading="crud.loading"
        :data="crud.data"
        size="small"
        style="width: 100%;"
        @selection-change="crud.selectionChangeHandler"
      >
        <el-table-column type="selection" width="55" />
        <el-table-column prop="id" label="id" />
        <el-table-column prop="foodNo" label="编号" />
        <el-table-column prop="foodName" label="名字" />
        <el-table-column prop="foodInstruction" label="介绍" />
        <el-table-column
          v-if="checkPer(['admin', 'sysFood:edit', 'sysFood:del'])"
          label="操作"
          width="150px"
          align="center"
        >
          <template slot-scope="scope">
            <udOperation :data="scope.row" :permission="permission" />
          </template>
        </el-table-column>
      </el-table>
      <!--分页组件-->
      <pagination />
    </div>
  </div>
</template>

<script>
import crudSysFood from "@/api/sysFood";
import CRUD, { presenter, header, form, crud } from "@crud/crud";
import rrOperation from "@crud/RR.operation";
import crudOperation from "@crud/CRUD.operation";
import udOperation from "@crud/UD.operation";
import pagination from "@crud/Pagination";

const defaultForm = {
  id: null,
  foodNo: null,
  foodName: null,
  foodInstruction: null
};
export default {
  name: "SysFoods",
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  cruds() {
    return CRUD({
      title: "foodlist",
      url: "api/sysFood",
      idField: "foodInstruction",
      sort: "foodInstruction,desc",
      crudMethod: { ...crudSysFood }
    });
  },
  data() {
    return {
      permission: {
        add: ["admin", "sysFood:add"],
        edit: ["admin", "sysFood:edit"],
        del: ["admin", "sysFood:del"]
      },
      rules: {
        id: [{ required: true, message: "不能为空", trigger: "blur" }],
        foodInstruction: [
          { required: true, message: "介绍不能为空", trigger: "blur" }
        ]
      },
      queryTypeOptions: [
        { key: "foodNo", display_name: "编号" },
        { key: "foodName", display_name: "名字" }
      ]
    };
  },
  methods: {
    // 钩子：在获取表格数据之前执行，false 则代表不获取数据
    [CRUD.HOOK.beforeRefresh]() {
      return true;
    }
  }
};
</script>

<style scoped></style>

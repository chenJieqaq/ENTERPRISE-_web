<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">deployeeNo</label>
        <el-input v-model="query.deployeeNo" clearable placeholder="deployeeNo" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">deployeeName</label>
        <el-input v-model="query.deployeeName" clearable placeholder="deployeeName" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <rrOperation :crud="crud" />
      </div>

      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <!--          <el-form-item label="id" prop="id">-->
          <!--            <el-input v-model="form.id" style="width: 370px;" />-->
          <!--          </el-form-item>-->
          <el-form-item label="员工编号">
            <el-input v-model="form.deployeeNo" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="员工名称">
            <el-input v-model="form.deployeeName" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="手机号">
            <el-input v-model="form.iphone" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="地址">
            <el-input v-model="form.address" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="email">
            <el-input v-model="form.email" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="性别">
            <el-input v-model="form.sex" style="width: 370px;" />
          </el-form-item>

        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button type="text" @click="crud.cancelCU">取消</el-button>
          <el-button :loading="crud.status.cu === 2" type="primary" @click="crud.submitCU">确认</el-button>
        </div>
      </el-dialog>
      <!--表格渲染-->
      <el-table ref="table" v-loading="crud.loading" :data="crud.data" size="small" style="width: 100%;" @selection-change="crud.selectionChangeHandler">
        <el-table-column type="selection" width="55" />

        <el-table-column prop="deployeeNo" label="员工编号" />
        <el-table-column prop="deployeeName" label="员工名称" />
        <el-table-column prop="iphone" label="iphone" />
        <el-table-column prop="address" label="地址" />
        <el-table-column prop="email" label="email" />
        <el-table-column prop="sex" label="性别" />

        <el-table-column v-if="checkPer(['admin','deployeeInformation:edit','deployeeInformation:del'])" label="操作" width="150px" align="center">
          <template slot-scope="scope">
            <udOperation
              :data="scope.row"
              :permission="permission"
            />
          </template>
        </el-table-column>
      </el-table>
      <!--分页组件-->
      <pagination />
    </div>
  </div>
</template>

<script>
import crudDeployeeInformation from '@/api/deployeeInformation'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'
// import {getToken} from "@/utils/auth";

const defaultForm = { id: null, deployeeNo: null, deployeeName: null, iphone: null, address: null, email: null, sex: null, createBy: null, createTime: null, updateBy: null, updateTime: null, isDelete: null }
export default {
  name: 'DeployeeInformation',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  cruds() {
    return CRUD({ title: 'Deployee_message', url: 'api/deployeeInformation', idField: 'id', sort: 'id,desc', crudMethod: { ...crudDeployeeInformation }})
  },
  data() {
    return {

      permission: {
        add: ['admin', 'deployeeInformation:add'],
        edit: ['admin', 'deployeeInformation:edit'],
        del: ['admin', 'deployeeInformation:del']
      },
      rules: {
        id: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ]
      },
      queryTypeOptions: [
        { key: 'deployeeNo', display_name: 'deployeeNo' },
        { key: 'deployeeName', display_name: 'deployeeName' }
      ]
    }
  },
  mounted() {

  },
  methods: {
    // 钩子：在获取表格数据之前执行，false 则代表不获取数据
    [CRUD.HOOK.beforeRefresh]() {
      return true
    }

  }
}
</script>

<style scoped>

</style>

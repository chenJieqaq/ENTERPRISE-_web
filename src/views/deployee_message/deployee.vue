<template>
    <div class="app-container">
      <!--工具栏-->
      <div class="head-container">
        <div v-if="crud.props.searchToggle">
          <!-- 搜索 -->
          <label class="el-form-item-label">deployeeNo</label>
          <el-input v-model="query.deployeeNo" clearable placeholder="deployeeNo" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
          <label class="el-form-item-label">iphone</label>
          <el-input v-model="query.iphone" clearable placeholder="iphone" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
          <label class="el-form-item-label">address</label>
          <el-input v-model="query.address" clearable placeholder="address" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
          <label class="el-form-item-label">sex</label>
          <el-input v-model="query.sex" clearable placeholder="sex" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
          <label class="el-form-item-label">deployeeName</label>
          <el-input v-model="query.deployeeName" clearable placeholder="deployeeName" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
          <rrOperation :crud="crud" />
        </div>
        <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
        <crudOperation :permission="permission" />
        <!--表单组件-->
        <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
          <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
            <!-- <el-form-item label="id" prop="id">
              <el-input v-model="form.id" style="width: 370px;" />
            </el-form-item> -->
            <el-form-item label="员工编号">
              <el-input v-model="form.deployeeNo" style="width: 80%;" />
            </el-form-item>
            <el-form-item label="手机号">
              <el-input v-model="form.iphone" style="width: 80%;" />
            </el-form-item>
            <el-form-item label="地址">
              <el-input v-model="form.address" style="width: 80%;" />
            </el-form-item>
            <el-form-item label="邮箱">
              <el-input v-model="form.email" style="width: 80%;" />
            </el-form-item>
            <el-form-item label="性别">
              <el-input v-model="form.sex" style="width: 80%;" />
            </el-form-item>
            <!-- <el-form-item label="创建者">
              <el-input v-model="form.createBy" style="width: 370px;" />
            </el-form-item> -->
            <!-- <el-form-item label="createTime">
              <el-input v-model="form.createTime" style="width: 370px;" />
            </el-form-item> -->
            <!-- <el-form-item label="updateBy">
              <el-input v-model="form.updateBy" style="width: 370px;" />
            </el-form-item>
            <el-form-item label="updateTime">
              <el-input v-model="form.updateTime" style="width: 370px;" />
            </el-form-item> -->
            <!-- <el-form-item label="isDelete">
              <el-input v-model="form.isDelete" style="width: 370px;" />
            </el-form-item> -->
            <el-form-item label="deployeeName">
              <el-input v-model="form.deployeeName" style="width: 80%;" />
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
          <!-- <el-table-column prop="id" label="id" /> -->
          <el-table-column prop="deployeeNo" label="deployeeNo" />
          <el-table-column prop="iphone" label="iphone" />
          <el-table-column prop="address" label="address" />
          <el-table-column prop="email" label="email" />
          <el-table-column prop="sex" label="sex" />
          <!-- <el-table-column prop="createBy" label="createBy" />
          <el-table-column prop="createTime" label="createTime" />
          <el-table-column prop="updateBy" label="updateBy" />
          <el-table-column prop="updateTime" label="updateTime" />
          <el-table-column prop="isDelete" label="isDelete" /> -->
          <el-table-column prop="deployeeName" label="deployeeName" />
          <el-table-column v-if="checkPer(['admin','depoloyee information:edit','depoloyee information:del'])" label="操作" width="150px" align="center">
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
  import crudDepoloyee from '@/api/bussiness/depoloyee_information'
  import CRUD, { presenter, header, form, crud } from '@crud/crud'
  import rrOperation from '@crud/RR.operation'
  import crudOperation from '@crud/CRUD.operation'
  import udOperation from '@crud/UD.operation'
  import pagination from '@crud/Pagination'
  
  const defaultForm = { id: null, deployeeNo: null, iphone: null, address: null, email: null, sex: null, createBy: null, createTime: null, updateBy: null, updateTime: null, isDelete: null, deployeeName: null }
  export default {
    name: 'Deployee',
    components: { pagination, crudOperation, rrOperation, udOperation },
    mixins: [presenter(), header(), form(defaultForm), crud()],
    cruds() {
      return CRUD({ title: 'deployee_information', url: 'api/depoloyee information', idField: 'id', sort: 'id,desc', crudMethod: { ...crudDepoloyee }})
    },
    data() {
      return {
        permission: {
          add: ['admin', 'depoloyee:add'],
          edit: ['admin', 'depoloyee:edit'],
          del: ['admin', 'depoloyee:del']
        },
        rules: {
          id: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ]
        },
        queryTypeOptions: [
          { key: 'deployeeNo', display_name: 'deployeeNo' },
          { key: 'iphone', display_name: 'iphone' },
          { key: 'address', display_name: 'address' },
          { key: 'sex', display_name: 'sex' },
          { key: 'deployeeName', display_name: 'deployeeName' }
        ]
      }
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
  
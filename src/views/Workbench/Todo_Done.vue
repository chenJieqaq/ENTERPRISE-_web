<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">标题</label>
        <el-input v-model="query.title" clearable placeholder="标题" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">类型</label>
        <el-input v-model="query.type" clearable placeholder="类型" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">被告知的员工名称</label>
        <el-input v-model="query.deployeeName" clearable placeholder="被告知的员工名称" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <rrOperation :crud="crud" />
      </div>
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="id" prop="id">
            <el-input v-model="form.id" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="标题">
            <el-select v-model="form.title" filterable placeholder="请选择">
              <el-option
                v-for="item in dict. HR_management"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="内容">
            <el-input v-model="form.content" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="截至时间">
            <el-input v-model="form.deadline" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="创建时间">
            <el-input v-model="form.createdAt" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="更新时间">
            <el-input v-model="form.updatedAt" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="状态">
            <el-input v-model="form.status" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="类型">
            <el-input v-model="form.type" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="被告知的员工编号">
            <el-input v-model="form.deployeeId" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="被告知的员工名称">
            <el-input v-model="form.deployeeName" style="width: 370px;" />
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
        <el-table-column prop="id" label="id" />
        <el-table-column prop="title" label="标题">
          <template slot-scope="scope">
            {{ dict.label. HR_management[scope.row.title] }}
          </template>
        </el-table-column>
        <el-table-column prop="content" label="内容" />
        <el-table-column prop="deadline" label="截至时间" />
        <el-table-column prop="createdAt" label="创建时间" />
        <el-table-column prop="updatedAt" label="更新时间" />
        <el-table-column prop="status" label="状态" />
        <el-table-column prop="type" label="类型" />
        <el-table-column prop="deployeeId" label="被告知的员工编号" />
        <el-table-column prop="deployeeName" label="被告知的员工名称" />
        <el-table-column v-if="checkPer(['admin','todoDone:edit','todoDone:del'])" label="操作" width="150px" align="center">
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
import crudTodoDone from '@/api/bussiness/todo_done'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, title: null, content: null, deadline: null, createdAt: null, updatedAt: null, status: null, type: null, deployeeId: null, deployeeName: null }
export default {
  name: 'TodoDone',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  dicts: [' HR_management'],
  cruds() {
    return CRUD({ title: 'todo_done', url: 'api/todoDone', idField: 'id', sort: 'id,desc', crudMethod: { ...crudTodoDone }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'todoDone:add'],
        edit: ['admin', 'todoDone:edit'],
        del: ['admin', 'todoDone:del']
      },
      rules: {
        id: [
          { required: true, message: 'id不能为空', trigger: 'blur' }
        ]
      },
      queryTypeOptions: [
        { key: 'title', display_name: '标题' },
        { key: 'type', display_name: '类型' },
        { key: 'deployeeName', display_name: '被告知的员工名称' }
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

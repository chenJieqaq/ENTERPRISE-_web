<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <!--      <div v-if="crud.props.searchToggle">-->
      <!--        &lt;!&ndash; 搜索 &ndash;&gt;-->
      <!--        <label class="el-form-item-label">title</label>-->
      <!--        <el-input v-model="query.title" clearable placeholder="title" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />-->
      <!--        <label class="el-form-item-label">type</label>-->
      <!--        <el-input v-model="query.type" clearable placeholder="type" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />-->
      <!--        <rrOperation :crud="crud" />-->
      <!--      </div>-->
      <!--      &lt;!&ndash;如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'&ndash;&gt;-->
      <!--      <crudOperation :permission="permission" />-->
      <!--表单组件-->
      <!--      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">-->
      <!--        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">-->
      <!--          <el-form-item label="id">-->
      <!--            <el-input v-model="form.id" style="width: 370px;" />-->
      <!--          </el-form-item>-->
      <!--          <el-form-item label="title">-->
      <!--            <el-select v-model="form.title" filterable placeholder="请选择">-->
      <!--              <el-option-->
      <!--                v-for="item in dict.title"-->
      <!--                :key="item.id"-->
      <!--                :label="item.label"-->
      <!--                :value="item.value"-->
      <!--              />-->
      <!--            </el-select>-->
      <!--          </el-form-item>-->
      <!--          <el-form-item label="content">-->
      <!--            <el-input v-model="form.content" style="width: 370px;" />-->
      <!--          </el-form-item>-->
      <!--          <el-form-item label="deadline">-->
      <!--            <el-input v-model="form.deadline" style="width: 370px;" />-->
      <!--          </el-form-item>-->
      <!--          <el-form-item label="createdAt">-->
      <!--            <el-input v-model="form.createdAt" style="width: 370px;" />-->
      <!--          </el-form-item>-->
      <!--          <el-form-item label="updatedAt">-->
      <!--            <el-input v-model="form.updatedAt" style="width: 370px;" />-->
      <!--          </el-form-item>-->
      <!--          <el-form-item label="status">-->
      <!--            <el-input v-model="form.status" style="width: 370px;" />-->
      <!--          </el-form-item>-->
      <!--          <el-form-item label="type">-->
      <!--            <el-input v-model="form.type" style="width: 370px;" />-->
      <!--          </el-form-item>-->
      <!--          <el-form-item label="isdelete">-->
      <!--            <el-input v-model="form.isdelete" style="width: 370px;" />-->
      <!--          </el-form-item>-->
      <!--        </el-form>-->
      <!--        <div slot="footer" class="dialog-footer">-->
      <!--          <el-button type="text" @click="crud.cancelCU">取消</el-button>-->
      <!--          <el-button :loading="crud.status.cu === 2" type="primary" @click="crud.submitCU">确认</el-button>-->
      <!--        </div>-->
      <!--      </el-dialog>-->

      <!--      黑色分割线-->
      <div class="separator" />
      <el-tabs v-model="activeTab" @tab-click="handleTabClick">
        <el-tab-pane label="待办" name="待办">
          <!--表格渲染-->
          <el-table ref="table" v-loading="crud.loading" :data="crud.data" size="small" style="width: 100%;" @selection-change="crud.selectionChangeHandler">
            <!--            <el-table-column type="selection" width="55" />-->
            <el-table-column prop="title" label="title">
              <template slot-scope="scope">
                {{ dict.label.title[scope.row.title] }}
              </template>
            </el-table-column>
            <el-table-column prop="content" label="content" />
            <el-table-column prop="deadline" label="deadline" />
            <el-table-column prop="type" label="type" />
          </el-table>
          <!--分页组件-->
          <pagination />
        </el-tab-pane>

        <el-tab-pane label="已办" name="已办">
          <!--表格渲染-->
          <el-table ref="table" v-loading="crud.loading" :data="crud.data" size="small" style="width: 100%;" @selection-change="crud.selectionChangeHandler">
            <!--            <el-table-column type="selection" width="55" />-->
            <el-table-column prop="title" label="title" />
            <el-table-column prop="content" label="content" />
            <el-table-column prop="deadline" label="deadline" />
            <el-table-column prop="type" label="type" />
          </el-table>
          <!--分页组件-->
          <pagination />
        </el-tab-pane>

        <el-tab-pane label="通知" name="通知">
          <notification />
        </el-tab-pane>

      </el-tabs>

    </div>
  </div>
</template>

<script>
import crudTodoDone from '@/api/todoDone'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'
import Notification from '@/views/Notification'

const defaultForm = { id: null, title: null, content: null, deadline: null, createdAt: null, updatedAt: null, deployNo: null, status: null, type: null, isdelete: null }
export default {
  name: 'TodoDone',
  // eslint-disable-next-line vue/no-unused-components
  components: { Notification, pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  dicts: ['title'],
  cruds() {
    return CRUD({ title: '工作台', url: 'api/todoDone', idField: 'id', sort: 'id,desc', crudMethod: { ...crudTodoDone }})
  },
  data() {
    return {
      activeTab: '待办',
      querys: {
        type: '待办',
        title: ''
      },
      permission: {
        add: ['admin', 'todoDone:add'],
        edit: ['admin', 'todoDone:edit'],
        del: ['admin', 'todoDone:del']
      },
      rules: {
      },
      queryTypeOptions: [
        { key: 'title', display_name: 'title' },
        { key: 'type', display_name: 'type' }
      ]
    }
  },
  // 钩子函数
  mounted() {
    console.log(this.querys.type)
    this.handleTabClick()
    this.crud.params = { type: this.querys.type }
    this.crud.toQuery()
  },
  methods: {
    // 钩子：在获取表格数据之前执行，false 则代表不获取数据
    [CRUD.HOOK.beforeRefresh]() {

    },

    handleTabClick() {
      // this.crud.params= {title:'人事管理'};
      // this.crud.toQuery();
      this.querys.type = this.activeTab
      console.log(this.activeTab)
      this.crud.params = { type: this.querys.type }
      this.crud.toQuery()
    }
  }
}
</script>

<style scoped>
.separator {
  border-bottom: 2px solid black;
  margin-top: 20px;
}
</style>

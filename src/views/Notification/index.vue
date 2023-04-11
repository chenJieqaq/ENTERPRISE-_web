<template>
  <div class="app-container">

    <!--表格渲染-->
    <el-table ref="table" v-loading="crud.loading" :data="crud.data" size="small" style="width: 100%;" @selection-change="crud.selectionChangeHandler">

      <el-table-column prop="title" label="通知标题" />
      <el-table-column prop="content" label="通知内容" />
      <el-table-column prop="cteatedAt" label="创建时间" />
      <el-table-column prop="updatedAt" label="更新时间" />

    </el-table>
    <!--分页组件-->
    <pagination />
  </div>
</template>

<script>
import crudNotification from '@/api/notification'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, title: null, content: null, cteatedAt: null, updatedAt: null, deployeeNo: null, deplpyeeName: null, cteatedby: null, updatedby: null }
export default {
  name: 'Notification',
  // eslint-disable-next-line vue/no-unused-components
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  cruds() {
    return CRUD({ title: 'notification', url: 'api/notification', idField: 'id', sort: 'id,desc', crudMethod: { ...crudNotification }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'notification:add'],
        edit: ['admin', 'notification:edit'],
        del: ['admin', 'notification:del']
      },
      rules: {
        id: [
          { required: true, message: '通知id不能为空', trigger: 'blur' }
        ]
      },
      queryTypeOptions: [
        { key: 'title', display_name: '通知标题' },
        { key: 'deployeeNo', display_name: '被告知的员工编号' }
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

<template>
  <div>
    <h2>Job Replacement Request</h2>
    <div class="steps">
      <el-steps :active="activeStep" finish-status="success">
        <el-step title="申请初始化" />
        <el-step title="部门经理审批" />
        <el-step title="HR审批" />
        <el-step title="直属总监审批" />
      </el-steps>
    </div>
    <div v-if="activeStep==0" class="form">
      <el-form :model="form" label-width="100px">
        <el-form-item label="Replacement Person">
          <el-input v-model="form.name" placeholder="请填写申请人姓名" />
        </el-form-item>
        <el-form-item label="Current Position">
          <el-input v-model="form.oldPosition" placeholder="目前职位" />
        </el-form-item>
        <el-form-item label="New Position">
          <el-input v-model="form.newPosition" placeholder="申请职位" />
        </el-form-item>
        <el-form-item label="原因">
          <el-input v-model="form.reason" type="textarea" placeholder="请填写申请原因" />
        </el-form-item>
      </el-form>
    </div>
    <div class="actions">
      <el-button
        v-if="activeStep === 0"
        type="primary"
        @click="submitForm"
      >Submit Request</el-button>
      <el-button v-if="activeStep !== 0" @click="prevStep">Previous Step</el-button>
      <el-button v-if="activeStep < 1" type="primary" @click="commitStep">确认</el-button>
      <el-button v-if="activeStep !== 3 && activeStep !== 0" type="primary" @click="nextStep">Next Step</el-button>
      <el-button v-if="activeStep === 3" type="success" @click="approve">Approve</el-button>
      <el-button v-if="activeStep === 3" type="danger" @click="reject">Reject</el-button>
    </div>
  </div>
</template>

<script>
import crudTodoDone from '@/api/todoDone.js'
export default {
  name: 'JobChange',
  data() {
    return {

      activeStep: 0,
      form: {
        name: '',
        oldPosition: '',
        newPosition: '',
        reason: ''
      }
    }
  },
  methods: {
    submitForm() {
      // Submit form data to backend
      // TODO: Implement this based on actual requirements
      this.activeStep += 1
    },
    prevStep() {
      this.activeStep -= 1
    },
    nextStep() {
      this.activeStep += 1
    },
    commitStep() {
      this.activeStep += 1
      crudTodoDone.add({
        title: '岗位调动',
        // eslint-disable-next-line no-undef
        content: '姓名' + form.name + '  ' + this.form.oldPosition + '>>' + this.form.newPosition + this.form.newPosition,
        deployeeNo: '3',
        type: '待办'
      })
    },
    approve() {
      // Approve request at HR or Director level
      // TODO: Implement this based on actual requirements
      this.$message.success('Approved')
    },
    reject() {
      // Reject request at HR or Director level
      // TODO: Implement this based on actual requirements
      this.$message.error('Rejected')
    }
  }
}
</script>

<style>
.steps {
  margin: 20px 0;
}

.form {
  margin-bottom: 20px;
}

.actions {
  margin-top: 20px;
}
</style>

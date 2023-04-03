<template>
  <div>
    <h2>Job Replacement Request</h2>
    <div class="steps">
      <el-steps :active="activeStep" finish-status="success">
        <el-step title="Initiate Request"></el-step>
        <el-step title="Department Manager Approval"></el-step>
        <el-step title="HR Approval"></el-step>
        <el-step title="Director Approval"></el-step>
      </el-steps>
    </div>
    <div class="form">
      <el-form :model="form" label-width="100px">
        <el-form-item label="Replacement Person">
          <el-input v-model="form.name" placeholder="Enter the name of the replacement person"></el-input>
        </el-form-item>
        <el-form-item label="Current Position">
          <el-input v-model="form.oldPosition" placeholder="Enter the current position"></el-input>
        </el-form-item>
        <el-form-item label="New Position">
          <el-input v-model="form.newPosition" placeholder="Enter the new position"></el-input>
        </el-form-item>
        <el-form-item label="Reason for Replacement">
          <el-input v-model="form.reason" type="textarea" placeholder="Enter the reason for replacement"></el-input>
        </el-form-item>
      </el-form>
    </div>
    <div class="actions">
      <el-button type="primary" v-if="activeStep === 0" @click="submitForm">Submit Request</el-button>
      <el-button v-if="activeStep !== 0" @click="prevStep">Previous Step</el-button>
      <el-button type="primary" v-if="activeStep !== 3" @click="nextStep">Next Step</el-button>
      <el-button type="success" v-if="activeStep === 3" @click="approve">Approve</el-button>
      <el-button type="danger" v-if="activeStep === 3" @click="reject">Reject</el-button>
    </div>
  </div>
</template>

<script>
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

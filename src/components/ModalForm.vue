<template>
    <el-card class="modal-wrap" shadow="never">
      <el-button type="primary" @click="dialogVisible = true">New contact</el-button>

      <el-dialog class="dialog" :visible.sync="dialogVisible" @close="onClose">
        <el-form :model="form" :data="list" :rules="rules" ref="addItemForm" :label-position="labelPosition" label-width="120px">
          <el-form-item label="Name" prop="name">
            <el-input v-model="form.name" autocomplete="off"/>
          </el-form-item>

          <el-form-item label="Phone number" prop="number">
            <el-input type="number" v-model="form.number" autocomplete="off"/>
          </el-form-item>

          <el-form-item label="Team leader" prop="leader">
            <el-select v-model="form.leader">
              <el-option v-for="item in list" :key="item.id" :label="item.name" :value="item.id"/>
            </el-select>
          </el-form-item>
          <div class="buttons">
            <el-button @click="onClose">Close</el-button>
            <el-button type="primary" @click="onSubmit">Save</el-button>
          </div>
        </el-form>
      </el-dialog>
    </el-card>
</template>

<script>
export default {
  name: 'ModalForm',
  props: {
    list: {
      type: Array,
      default: () => ({})
    }
  },
  data () {
    return {
      dialogVisible: false,
      labelPosition: 'top',
      form: {
        id: '',
        leader: '',
        name: '',
        number: '',
        children: []
      },
      rules: {
        name: [
          { required: true, message: 'Name required', trigger: 'blur' },
          { min: 2, message: 'Length should be at least 2 characters', trigger: 'change' }
        ],
        number: [
          { required: true, message: 'Phone number required', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    onSubmit () {
      this.$refs.addItemForm.validate((valid) => {
        if (valid) {
          this.$emit('submitForm', { ...this.form })
          this.$refs.addItemForm.resetFields()
          this.dialogVisible = false
        } else {
          return false
        }
      })
    },
    onClose () {
      this.$refs.addItemForm.resetFields()
      this.dialogVisible = false
    }
  }
}
</script>

<style scoped>
.modal-wrap {
  border: none;
}
.dialog {
  min-width: 300px;
}
.buttons {
  display: flex;
  flex-flow: row;
  justify-content: space-between;
}

@media (max-width: 750px) {
  .dialog {
    min-width: 90%;
  }
}
</style>

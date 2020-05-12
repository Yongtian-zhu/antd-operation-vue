<template>
  <a-modal
    title="新建组织"
    :width="800"
    v-model="visible"
    :confirmLoading="confirmLoading"
    @ok="handleOk"
    @cancel="handleCancel"
  >
    <a-form-model
      ref="ruleForm"
      :model="form"
      :rules="rules"
      :label-col="labelCol"
      :wrapper-col="wrapperCol"
    >

      <a-form-model-item label="上级组织" prop="superior">
        <a-select v-model="mdl.name" placeholder="请选择">
          <a-select-option value="shiwei">
            市委
          </a-select-option>
          <a-select-option value="gongan">
            公安局
          </a-select-option>
        </a-select>
      </a-form-model-item>

      <a-form-model-item ref="name" label="组织名称" prop="name">
        <a-input placeholder="请输入" v-model="mdl.id" @blur=" () => { $refs.name.onFieldBlur(); } " />
      </a-form-model-item>

      <a-form-model-item label="分类" prop="sort">
        <a-select v-model="form.sort" placeholder="请选择">
          <a-select-option value="zuzhi">
            组织
          </a-select-option>
          <a-select-option value="bumen">
            部门
          </a-select-option>
        </a-select>
      </a-form-model-item>

      <a-form-model-item label="备注" prop="desc">
        <a-input v-model="form.desc" type="textarea" />
      </a-form-model-item>
      <!-- <a-form-model-item :wrapper-col="{ span: 14, offset: 4 }">
        <a-button style="margin-left: 10px;" >
          取消
        </a-button>
        <a-button type="primary" @click="onSubmit">
          创建
        </a-button>
      </a-form-model-item> -->
    </a-form-model>
  </a-modal>
</template>

<script>
import { getPermissions } from '@/api/manage'
import { actionToObject } from '@/utils/permissions'
import pick from 'lodash.pick'

export default {
  name: 'AddModal',
  data () {
    return {
      labelCol: {
        xs: { span: 24 },
        sm: { span: 5 }
      },
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 16 }
      },
      visible: false,
      confirmLoading: false,
      mdl: {},

      // form: this.$form.createForm(this),
      form: {
        name: '',
        superior: undefined,
        sort: undefined,
        desc: ''
      },
      rules: {
        name: [
          { required: true, message: '请输入', trigger: 'blur' }
          // { min: 3, max: 5, message: 'Length should be 3 to 5', trigger: 'blur' }
        ],
        superior: [{ required: true, message: '请选择', trigger: 'change' }],
        sort: [{ required: true, message: '请选择', trigger: 'change' }]
      },

      permissions: []
    }
  },
  created () {
    this.loadPermissions()
  },
  methods: {
    onSubmit () {
      this.$refs.ruleForm.validate(valid => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    add () {
      this.edit({ id: 0 })
    },
    edit (record) {
      this.mdl = Object.assign({}, record)
      this.visible = true

      // 有权限表，处理勾选
      if (this.mdl.permissions && this.permissions) {
        // 先处理要勾选的权限结构
        const permissionsAction = {}
        this.mdl.permissions.forEach(permission => {
          permissionsAction[permission.permissionId] = permission.actionEntitySet.map(entity => entity.action)
        })
        // 把权限表遍历一遍，设定要勾选的权限 action
        this.permissions.forEach(permission => {
          permission.selected = permissionsAction[permission.id] || []
        })
      }

      this.$nextTick(() => {
        this.form.setFieldsValue(pick(this.mdl, 'id', 'name', 'status', 'describe'))
      })
      console.log('this.mdl', this.mdl)
    },
    close () {
      this.$emit('close')
      this.visible = false
    },
    handleOk () {
      const _this = this
      // 触发表单验证
      this.form.validateFields((err, values) => {
        // 验证表单没错误
        if (!err) {
          console.log('form values', values)

          _this.confirmLoading = true
          // 模拟后端请求 2000 毫秒延迟
          new Promise((resolve) => {
            setTimeout(() => resolve(), 2000)
          }).then(() => {
            // Do something
            _this.$message.success('保存成功')
            _this.$emit('ok')
          }).catch(() => {
            // Do something
          }).finally(() => {
            _this.confirmLoading = false
            _this.close()
          })
        }
      })
    },
    handleCancel () {
      this.close()
    },
    onChangeCheck (permission) {
      permission.indeterminate = !!permission.selected.length && (permission.selected.length < permission.actionsOptions.length)
      permission.checkedAll = permission.selected.length === permission.actionsOptions.length
    },
    onChangeCheckAll (e, permission) {
      Object.assign(permission, {
        selected: e.target.checked ? permission.actionsOptions.map(obj => obj.value) : [],
        indeterminate: false,
        checkedAll: e.target.checked
      })
    },
    loadPermissions () {
      const that = this
      getPermissions().then(res => {
        const result = res.result
        that.permissions = result.map(permission => {
          const options = actionToObject(permission.actionData)
          permission.checkedAll = false
          permission.selected = []
          permission.indeterminate = false
          permission.actionsOptions = options.map(option => {
            return {
              label: option.describe,
              value: option.action
            }
          })
          return permission
        })
      })
    }

  }
}
</script>

<style scoped>

</style>

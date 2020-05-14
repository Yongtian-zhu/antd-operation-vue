<template>
  <a-modal
    title="1111111"
    :width="640"
    :visible="visible"
    :confirmLoading="confirmLoading"
    @cancel="handleCancel"
  >
    <a-spin :spinning="confirmLoading">
      <a-form
        :labelCol="labelCol"
        :wrapperCol="wrapperCol"
        :model="form"
        :form="form">
        <a-form-item label="资源ID">
          <a-input placeholder="请输入" v-decorator="['id', {rules: [{required: true}]}]" />
        </a-form-item>

        <a-form-item label="资源名称">
          <a-input placeholder="请输入" v-decorator="['name', {rules: [{required: true }]}]" />
        </a-form-item>

        <a-form-item label="资源类型" prop="resource">
          <a-radio-group v-decorator="['radio', {rules: [{required: true}]}]">
            <a-radio value="a">
              接口
            </a-radio>
            <a-radio value="b">
              页面
            </a-radio>
          </a-radio-group>
        </a-form-item>

        <a-form-item label="资源名称" prop="name">
          <a-radio-group v-decorator="['radio-user', {rules: [{required: true}]}]">
            <a-radio value="1">
              匿名
            </a-radio>
            <a-radio value="2">
              登录
            </a-radio>
            <a-radio value="3">
              授权
            </a-radio>
          </a-radio-group>
        </a-form-item>

        <a-form-item label="资源地址">
          <a-input placeholder="请输入" v-decorator="['address', {rules: [{required: true}]}]" />
        </a-form-item>

        <a-form-item label="备注">
          <a-input v-decorator="['desc']" type="textarea" />
        </a-form-item>
      </a-form>
    </a-spin>
  </a-modal>
</template>

<script>
import pick from 'lodash.pick'

const stepForms = [
  ['name', 'desc'],
  ['target', 'template', 'type'],
  ['time', 'frequency']
]

export default {
  name: 'StepByStepModal',
  data () {
    return {
      labelCol: {
        xs: { span: 24 },
        sm: { span: 7 }
      },
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 13 }
      },
      visible: false,
      confirmLoading: false,
      currentStep: 0,
      mdl: {},

      form: this.$form.createForm(this)
    }
  },
  methods: {
    edit (record) {
      this.visible = true
      const { form: { setFieldsValue } } = this
      this.$nextTick(() => {
        setFieldsValue(pick(record, []))
      })
    },
    handleNext (step) {
      const { form: { validateFields } } = this
      const currentStep = step + 1
      if (currentStep <= 2) {
        // stepForms
        validateFields(stepForms[ this.currentStep ], (errors, values) => {
          if (!errors) {
            this.currentStep = currentStep
          }
        })
        return
      }
      // last step
      this.confirmLoading = true
      validateFields((errors, values) => {
        console.log('errors:', errors, 'val:', values)
        if (!errors) {
          console.log('values:', values)
          setTimeout(() => {
            this.confirmLoading = false
            this.$emit('ok', values)
          }, 1500)
        } else {
          this.confirmLoading = false
        }
      })
    },
    backward () {
      this.currentStep--
    },
    handleCancel () {
      // clear form & currentStep
      this.visible = false
      this.currentStep = 0
    }
  }
}
</script>

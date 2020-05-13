<template>
  <a-modal
    title="添加/修改资源"
    :width="640"
    :visible="visible"
    :confirmLoading="confirmLoading"
    @ok="handleSubmit"
    @cancel="handleCancel"
  >
    <a-spin :spinning="confirmLoading">
      <a-form
        :labelCol="labelCol"
        :wrapperCol="wrapperCol"
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

export default {
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

      form: this.$form.createForm(this)
    }
  },
  methods: {
    add () {
      this.visible = true
    },
    handleSubmit () {
      const { form: { validateFields } } = this
      this.confirmLoading = true
      validateFields((errors, values) => {
        if (!errors) {
          console.log('values', values)
          setTimeout(() => {
            this.visible = false
            this.confirmLoading = false
            this.$emit('ok', values)
          }, 1500)
        } else {
          this.confirmLoading = false
        }
      })
    },
    handleCancel () {
      this.visible = false
    }
  }
}
</script>

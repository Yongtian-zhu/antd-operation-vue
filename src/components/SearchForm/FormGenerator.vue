<template>
  <div>
    <a-form layout="inline" :form="form" @submit="handleSubmit">
      <a-row :gutter="48">
        <a-col
          :md="field.cols||8"
          :sm="24"
          v-for="(field, index) in defaultFieldsConfig"
          :key="index"
        >
          <component
            :key="index"
            :is="field.fieldType"
            :label="field.label"
            :multiple="field.multiple"
            v-bind="field"
            :options="field.options"
            :ref="field.name"
            :initValue="formData[field.name]"
          >
          </component>
        </a-col>
        <template v-if="advanced">
          <a-col
            :md="field.cols||8"
            :sm="24"
            v-for="(field, index) in advancedFieldsConfig"
            :key="index+2"
          >
            <component
              :key="index"
              :is="field.fieldType"
              :label="field.label"
              :multiple="field.multiple"
              v-bind="field"
              :options="field.options"
              :ref="field.name"
              :initValue="formData[field.name]"
            >
            </component>
          </a-col>
        </template>
        <a-col :md="!advanced && 8 || 24" :sm="24">
          <span
            class="table-page-search-submitButtons"
            :style="advanced && { float: 'right', overflow: 'hidden' } || {} ">
            <a-button type="primary" html-type="submit">{{ onSubmitText }}</a-button>
            <a-button style="margin-left: 8px" @click="handleReset">{{ onResetText }}</a-button>
            <a v-if="isAdvanced" @click="toggleAdvanced" style="margin-left: 8px">{{ advanced ? '收起' : '展开' }}
              <a-icon
                :type="advanced ? 'up' : 'down'"/>
            </a>
          </span>
        </a-col>
      </a-row>
    </a-form>
  </div>
</template>
<script>
  import SelectList from './basicComponent/SelectList'
  import TextInput from './basicComponent/TextInput'
  import DatePicker from './basicComponent/DatePicker'
  import RangePicker from './basicComponent/RangePicker'
  import NumberInput from './basicComponent/NumberInput'

  export default {
    name: 'FormGenerator',
    components: { SelectList, TextInput, DatePicker, RangePicker, NumberInput },
    props: [ 'config', 'value' ],
    data () {
      return {
        formData: this.value || {},
        onSubmitText: this.config.buttons.onSubmitText || '查询',
        onResetText: this.config.buttons.onResetText || '重置',
        fieldsConfig: this.config.fieldsConfig,
        defaultFieldsConfig: this.config.fieldsConfig,
        advancedFieldsConfig: [],
        advanced: false, // 搜索 展开/关闭
        isAdvanced: false // 是否显示展开/关闭 的按钮
      }
    },
    beforeCreate () {
      this.form = this.$form.createForm(this, { name: 'validate_search_forms' })
    },
    created () {
      if (Array.isArray(this.fieldsConfig) && this.fieldsConfig.length > 2) {
        this.defaultFieldsConfig = this.fieldsConfig.slice(0, 2)
        this.advancedFieldsConfig = this.fieldsConfig.slice(2)
        this.isAdvanced = true
      }
    },
    methods: {
      handleSubmit (e) {
        e.preventDefault()
        const that = this
        this.form.validateFields((err, values) => {
          if (!err) {
            // console.log('values of form: ', values)
            that.$emit('submit', values)
          }
        })
      },
      handleReset () {
        this.form.resetFields()
        this.formData = {}
      },
      toggleAdvanced () {
        this.advanced = !this.advanced
      }
    }
  }
</script>

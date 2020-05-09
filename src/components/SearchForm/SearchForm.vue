<template>
  <div class="search-form">
    <a-fragment>
      <div class="tableListForm">{{renderForm()}}</div>
      <div class="tableListOperator">
        <div class="tableListleft">
          {this.$attrs.rightButton}
        </div>
        <div class="tableListRight">
          <a-button type="primary" @click="handleSearch">
            {this.$attrs.SearchText || "查询"}
          </a-button>
          <a-button style=" marginLeft: 8" @click="handleFormReset">
            重置
          </a-button>
        </div>
      </div>
    </a-fragment>
  </div>
</template>

<script>
  // 引入文件, 如:import 组件名 from '组件路径'
  // import { connect } from '@/dva'
  import { Form, Row, Col, Select, Icon, Input, DatePicker, InputNumber } from 'antd'
  import '@/commonTableList.less'

  export default {
    // import需要注入到对象中才能使用
    components: {

    },
    data () { // 存放数据
      return {

      }
    },
    // 监听属性
    computed: {},
    // 监控data中的数据变化
    watch: {},
    // 方法集合

    methods: {
      handleSearch (e) {
        e.preventDefault()
      this.$attrs.form.validateFields((err, fieldsValue) => {
        if (err) return
        const values = { ...fieldsValue }
        this.formValues = values
        this.$attrs.dispatch({
          type: `${this.$attrs.type}`,
          payload: values
        })
      })
    },

    handleFormReset () {
      const {
        form,
        dispatch
      } = this.$attrs
      form.resetFields()
      this.formValues = {}
      dispatch({
        type: `${this.$attrs.type}`,
        payload: {}
      })
    },

    switchItem (item) {
      const type = item.type
      const placeholder = item.placeholder

      switch (type) {
        case 'number':
          return <InputNumber placeholder="请输入" min={0} style={{
            width: '100%'
          }} />
        // break;

        case 'text':
          return <Input placeholder="请输入" />
        // break;

        case 'date_picker':
          return <DatePicker placeholder="请选择" allowClear={false} style={{
            width: '100%'
          }} />
        // break;

        case 'range_picker':
          return <RangePicker style={{
            width: '100%'
          }} allowClear={false} />
        // break;

        case 'select_multiple':
          return <Select mode="multiple" placeholder={placeholder} style={{
            width: '100%'
          }}>
            {item.options.map((option, index) => {
              return <Option key={index} value={option.value}>
                  {option.text}
                </Option>
            })}
          </Select>

        case 'select':
          return <Select placeholder="请选择" showSearch optionFilterProp="children" filterOption={(input, option) => option.props.children.toLowerCase().indexOf(input.toLowerCase()) >= 0} style={{
            width: '100%'
          }}>
            {}
            {item.options && item.options.map((option, index) => {
              return <Option key={index} value={option.value}>
                    {option.value}
                  </Option>
            })}
          </Select>

        default:
          return <Input placeholder="请选择" autoComplete="off" />
        // break;
      }
    },

    toggleForm () {
      const {
        expandForm
      } = this.$data
      this.expandForm = !expandForm
    },

    renderSimpleForm () {
      const {
        form: {
          getFieldDecorator
        },
        formList
      } = this.$attrs
      const simpleForm = formList.slice(0, 3)
      return <Form onSubmit={this.handleSearch} layout="inline">
        <Row gutter={{
          md: 7,
          lg: 24,
          xl: 48
        }}>
          {simpleForm.map((item, index) => {
            return <Col md={7} sm={24} key={index}>
                <FormItem label={<span class="labelText">{item.label}</span>}>
                  {getFieldDecorator(item.field)(this.switchItem(item))}
                </FormItem>
              </Col>
          })}
          {formList.length > 3 ? <Col md={3} sm={24}>
            <span class="submitButtons">
              <a onClick={this.toggleForm}>
                展开 <Icon type="down" />
              </a>
            </span>
          </Col> : ' ' }
        </Row>
      </Form>
    },

    renderAdvancedForm () {
      const {
        form: {
          getFieldDecorator
        },
        formList
      } = this.$attrs
      const simpleForm = formList.slice(0, 3)
      const AdvancedForm = formList.slice(3, formList.length)
      return <Form onSubmit={this.handleSearch} layout="inline">
        <Row gutter={{
          md: 7,
          lg: 24,
          xl: 48
        }}>
          {simpleForm.map((item, index) => {
            return <Col md={7} sm={24} key={index}>
                <FormItem label={<span class="labelText">{item.label}</span>}>
                  {getFieldDecorator(item.field)(this.switchItem(item))}
                </FormItem>
              </Col>
          })}

          <Col md={3} sm={24}>
            <span class="submitButtons">
              <a style={{
                marginLeft: 8
              }} onClick={this.toggleForm}>
                收起 <Icon type="up" />
              </a>
            </span>
          </Col>
        </Row>
        <Row gutter={{
          md: 7,
          lg: 24,
          xl: 48
        }}>
          {AdvancedForm.map((item, index) => {
            return <Col md={7} sm={24} key={index}>
                <FormItem label={<span class="labelText">{item.label}</span>}>
                  {getFieldDecorator(item.field)(this.switchItem(item))}
                </FormItem>
              </Col>
          })}
        </Row>
      </Form>
    },

    renderForm () {
      const {
        expandForm
      } = this.$data
      return expandForm ? this.renderAdvancedForm() : this.renderSimpleForm()
    }
  },

    // 声明周期 - 创建完成(可访问当前this实例)
    created () {},
    // 声明周期 - 挂载完成(可访问DOM元素)
    mounted () {}
  }

</script>

<style scoped>
/* @import url(); 引入样式 */

</style>

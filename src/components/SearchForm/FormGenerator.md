# FormGenerator 

构建动态表单



引用方式：

```javascript
import FormGenerator from '@/components/SearchForm/FormGenerator'

export default {
    components: {
        FormGenerator
    }
}
```



## 使用的Dome

```html
<template>
  <a-card :bordered="false">
    <div class="table-page-search-wrapper">
      <form-generator
        :config="config"
        @submit="getFormData"
        v-model="formData"
      >
      </form-generator>
    </div>
  </a-card>
</template>

<script>
  import FormGenerator from '@/components/SearchForm/FormGenerator'

  export default {
    name: 'TestDome',
    components: { FormGenerator },
    created () {
    },
    data () {
      return {
        // 初始化默认值
        formData: {
          orderCode: '测试',
          orderType: '定单类型3',
          area: ['区域2', '区域3'],
          staff: 22
        },
        // 表单渲染配置
        config: {
          fieldsConfig: [
            {
              name: 'orderCode',
              label: '定单编码',
              fieldType: 'TextInput',
              cols: 8
            },
            {
              name: 'orderType',
              label: '定单类型',
              fieldType: 'SelectList',
              options: [
                { label: '定单类型1', value: '定单类型1' },
                { label: '定单类型2', value: '定单类型2' },
                { label: '定单类型3', value: '定单类型3' }
              ],
              cols: 8
            },
            {
              name: 'beginTime',
              label: '开始时间',
              fieldType: 'DatePicker',
              cols: 8
            },
            {
              name: 'endTime',
              label: '时间段',
              fieldType: 'RangePicker',
              cols: 8
            },
            {
              name: 'area',
              label: '区域',
              fieldType: 'SelectList',
              options: [
                { label: '区域1', value: '区域1' },
                { label: '区域2', value: '区域2' },
                { label: '区域3', value: '区域3' }
              ],
              multiple: 'multiple',
              cols: 8
            },
            {
              name: 'staff',
              label: '人员数',
              fieldType: 'NumberInput',
              cols: 8,
            }
          ],
          buttons: {
            onSubmitText: '查询',
            onResetText: '重置'
          }
        }
      }
    },
    methods: {
      // 获取表单的值
      getFormData (values) {
        console.log(values)
      },
    }
  }
</script>

```



## API


参数 | 说明 | 类型 | 默认值
----|------|-----|------
config | 配置参数 | Object | -
v-model | 表单初始化的值 | Object | {}
submit | 获取表单的值 | Function(fieldValues) | -
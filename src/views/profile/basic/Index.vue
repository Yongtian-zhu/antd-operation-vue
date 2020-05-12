<template>
  <!-- 网络安全 -->
  <a-card :bordered="false">
    <a-form
      :label-col="labelCol"
      :wrapper-col="wrapperCol"
      style="margin-top: 8px">
      <span class="title">网络安全</span>
      <div class="form_group">
        <a-form-item label="启用白名单">
          <a-checkbox>启用</a-checkbox>
        </a-form-item>
        <a-form-item label="IP/域名白名单">
          <a-textarea
            placeholder="请输入"
          />
        </a-form-item>
        <a-form-item label="启用访问日志">
          <a-checkbox>启用</a-checkbox>
        </a-form-item>
      </div>
    </a-form>
  </a-card>
</template>

<script>
import { PageView } from '@/layouts'
import { STable } from '@/components'
import DetailList from '@/components/tools/DetailList'

export default {
  components: {
    PageView,
    DetailList,
    STable
  },
  data () {
    return {
      labelCol: {
        xs: { span: 24 },
        sm: { span: 8 },
        md: { span: 8 },
        lg: { span: 4 },
        xxl: { span: 3 }
      },
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 12 },
        md: { span: 12 },
        lg: { span: 12 },
        xxl: { span: 8 }
      }
    }
  }
}
</script>

<style lang="less" scoped>
  .title {
  font-weight: 600;
  color: rgba(0, 0, 0, 0.85);
  font-size: 16px;
  padding-left: 20px;
  font-family: 'PingFang SC Bold', 'PingFang SC';
  line-height: 24px;
}

.form_group {
  margin-top: 15px;
}

.input_trem {
  padding-left: 8px;
}

.tableListOperator {
  margin-top: 16px;
  margin-bottom: 16px;

  button {
    margin-right: 8px;
  }
}

</style>

<template>
  <!-- 账户安全 -->
  <a-card :bordered="false">
    <a-form
      :label-col="labelCol"
      :wrapper-col="wrapperCol"
      style="margin-top: 8px">
      <span class="title">密码策略</span>
      <div class="form_group">
        <a-form-item label="启用复杂度">
          <a-checkbox>启用</a-checkbox>
        </a-form-item>

        <a-form-item label="密码长度最小值">
          <a-input-number :min="6" />
          <span class="input_trem">个字符</span>
        </a-form-item>

        <a-form-item label="密码基本使用期限">
          <a-input-number :min="1" />
          <span class="input_trem">天</span>
        </a-form-item>

        <a-form-item label="复杂度要求">
          <a-checkbox-group>

            <a-checkbox value="1">
              含字母
            </a-checkbox>
            <a-checkbox value="2">
              含数字
            </a-checkbox>
            <a-checkbox value="3">
              含特殊字符
            </a-checkbox>
          </a-checkbox-group>
        </a-form-item>
      </div>

      <span class="title">锁定策略</span>
      <div class="form_group">
        <a-form-item label="启用锁定">
          <a-checkbox>启用</a-checkbox>
        </a-form-item>
        <a-form-item label="锁定阈值">
          <a-input-number :min="1" />
          <span class="input_trem">次登陆失败</span>
        </a-form-item>
        <a-form-item label="锁定时间">
          <a-input-number :min="1" />
          <span class="input_trem">分钟</span>
        </a-form-item>
      </div>

      <span class="title">登陆日志</span>
      <div class="form_group">
        <a-form-item label="启用登陆日志">
          <a-checkbox>启用</a-checkbox>
        </a-form-item>
      </div>
      <a-form-item style="text-align:center">
        <a-button type="primary" >保存</a-button>
      </a-form-item>
    </a-form>
  </a-card>
</template>

<script>
export default {
  name: 'AccountSecurity',
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

<template>
  <!-- 账户安全 -->
  <a-card :bordered="false">
    <a-form style="margin-top: 8px">
      <span class="title">密码策略</span>
      <div class="form_group">
        <a-form-item label="启用复杂度">
          <a-checkbox>启用</a-checkbox>
        </a-form-item>

        <a-form-item label="密码长度最小值">
          <a-input min="6" />
          <span class="input_trem">个字符</span>
        </a-form-item>

        <a-form-item label="密码基本使用期限">
          <a-input min="1" />
          <span class="input_trem">天</span>
        </a-form-item>

        <a-form-item label="复杂度要求">
          <a-checkbox value="1">
            含字母
          </a-checkbox>
          <a-checkbox value="2">
            含数字
          </a-checkbox>
          <a-checkbox value="3">
            含特殊字符
          </a-checkbox>
        </a-form-item>
      </div>

      <span class="title">锁定策略</span>
      <div class="form_group">
        <a-form-item label="启用锁定">
          <a-checkbox>启用</a-checkbox>
        </a-form-item>
      </div>
    </a-form>
  </a-card>
</template>

<script>
export default {
  name: 'AccountSecurity',
  data () {
    return {

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

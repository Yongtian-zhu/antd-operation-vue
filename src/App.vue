<template>
  <a-config-provider :locale="locale">
    <div id="app">
      <Header></Header>
      <router-view/>
    </div>
  </a-config-provider>
</template>

<script>
import zhCN from 'ant-design-vue/lib/locale-provider/zh_CN'
import { AppDeviceEnquire } from '@/utils/mixin'

import Header from '@/components/Header/Header.vue'

export default {
  mixins: [AppDeviceEnquire],
  components: {
    Header
  },
  data () {
    return {
      locale: zhCN
    }
  },
  mounted () {

  }
}
</script>

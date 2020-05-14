<template>
  <a-from-item :label="label">
    <a-cascader
      :options="options"
      :placeholder="请选择"
      v-decorator="[name,{ initialValue: initValue }]"
    />
  </a-from-item>
</template>

<script>
export default {
  name: 'Cassader',
  props: ['label', 'name', 'initValue', 'value'],
  data () {
    return {
    }
  }
}
</script>

<style>

</style>

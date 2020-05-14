<template>
  <a-form-item :label="label">
    <a-input
      :placeholder="placeholder || '请输入'"
      v-decorator="[name,{ initialValue: initValue }]"
    />
  </a-form-item>
</template>

<script>
  export default {
    name: 'TextInput',
    props: ['placeholder', 'label', 'name', 'initValue']
  }
</script>

<style scoped>
</style>

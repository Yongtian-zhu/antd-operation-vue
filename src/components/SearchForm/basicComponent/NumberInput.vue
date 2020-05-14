<template>
  <a-form-item :label="label">
    <a-input-number
      :style="{width: '100%'}"
      :placeholder="placeholder"
      v-decorator="[name,{ initialValue: initValue }]"
    />
  </a-form-item>
</template>

<script>
  export default {
    name: 'NumberInput',
    props: ['placeholder', 'label', 'name', 'initValue']
  }
</script>

<style scoped>
</style>

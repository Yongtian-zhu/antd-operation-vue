<template>
  <a-form-item :label="label">
    <a-date-picker
      :style="{width: '100%'}"
      placeholder="请选择"
      v-decorator="[name,{ initialValue: initValue }]"
    />
  </a-form-item>
</template>

<script>
  export default {
    props: ['label', 'name', 'initValue']
  }
</script>

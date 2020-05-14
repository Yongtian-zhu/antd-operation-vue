<template>
  <a-form-item :label="label">
    <a-range-picker
      :style="{width: '100%'}"
      :placeholder="placeholder || ['开始时间','结束时间']"
      v-decorator="[name,{ initialValue: initValue }]"
    />
  </a-form-item>
</template>

<script>
  export default {
    name: 'RangePicker',
    props: ['label', 'name', 'initValue']
  }
</script>

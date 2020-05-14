<template>
  <a-form-item :label="label">
    <a-select
      placeholder="请选择"
      :style="{width: '100%'}"
      :mode="multiple"
      v-decorator="[name,{ initialValue: initValue }]"
    >
      <a-select-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
      >
        {{ item.value }}
      </a-select-option>
    </a-select>
  </a-form-item>
</template>

<script>
  export default {
    name: 'SelectList',
    props: ['multiple', 'options', 'name', 'label', 'initValue']
  }
</script>

<template>
  <a-card :bordered="false">
    <form-generator
      :config="config"
      v-model="formData"
    >
    </form-generator>
  </a-card>
</template>

<script>
import FormGenerator from '@/components/SearchForm/FormGenerator'

export default {
  name: 'User',
  components: {
    FormGenerator
  },
  data () {
    return {
    }
  },
  methods: {}

}
</script>

<style>

</style>

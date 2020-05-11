<template>
  <div class="search-from">
    <div class="tableListOperator">
      <div class="table-list-left"></div>
      <div class="table-list-right">
        <a-button type="primary" @click="handleSearch">查询</a-button>
        <a-button @click="handleFormReset">重置</a-button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SearchFrom'

}
</script>

<style scoped>

</style>

<template>
  <a-card :bordered="false">
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="48">
          <a-col :md="8" :sm="24">
            <a-form-item label="组织">
              <a-input placeholder="请输入"/>
            </a-form-item>
          </a-col>
          <a-col :md="8" :sm="24">
            <a-form-item label="状态">
              <a-select placeholder="请选择" >
                <a-select-option value="0">启用</a-select-option>
                <a-select-option value="1">禁用</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :md="8" :sm="24">
            <a-form-item label="上级组织">
              <a-select placeholder="请选择" >
                <a-select-option value="0">市公安局</a-select-option>
                <a-select-option value="1">市委</a-select-option>
                <a-select-option value="2">省厅</a-select-option>
                <a-select-option value="3">中央指挥</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <!-- 按钮 -->
          <a-col :md="12" :sm="24" >
            <span>
              <a-button @click="$refs.createModal.add()" icon="plus" type="primary">
                新建
              </a-button>
              <a-button type="primary" style="margin-left: 10px">
                删除
              </a-button>
              <a-button style="margin-left: 10px">导出列表</a-button>
            </span>
          </a-col>

          <a-col :md="12" :sm="24" :style="{ textAlign: 'right' }">
            <span class="table-page-search-submitButtons">
              <a-button type="primary">查询</a-button>
              <a-button style="margin-left: 10px">重置</a-button>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>

    <s-table
      ref="table"
      size="default"
      rowKey="key"
      :columns="columns"
      :data="loadData"
      :alert="{ show: true, clear: true }"
      :rowSelection="{ selectedRowKeys: this.selectedRowKeys, onChange: this.onSelectChange }"
    >
      <template v-for="(col, index) in columns" v-if="col.scopedSlots" :slot="col.dataIndex" slot-scope="text, record">
        <div :key="index">
          <a-input
            v-if="record.editable"
            style="margin: -5px 0"
            :value="text"
            @change="e => handleChange(e.target.value, record.key, col, record)"
          />
          <template v-else>{{ text }}</template>
        </div>
      </template>

      <template slot="action" slot-scope="text, record">
        <div class="editable-row-operations">
          <span v-if="record.editable">
            <a @click="() => save(record)">保存</a>
            <a-divider type="vertical" />
            <a-popconfirm title="真的放弃编辑吗?" @confirm="() => cancel(record)">
              <a>取消</a>
            </a-popconfirm>
          </span>
          <!-- 操作 -->
          <span v-else slot="action" slot-scope="text, record">
            <a class="alter" @click="handleEdit(record)">修改</a>
            <a-divider type="vertical" />
            <a class="disable" @click="() => disable(record)">禁用</a>
          </span>
        </div>
      </template>
    </s-table>
    <!-- 新建 -->
    <add-modal ref="createModal" @ok="handleOk"></add-modal>
    <!-- 修改 -->
    <alter-modal ref="modal" @ok="handleOk"></alter-modal>

  </a-card>
</template>

<script>
import { STable } from '@/components'
import AlterModal from './modules/AlterModal'
import AddModal from './modules/AddModal'

export default {
  name: 'BaseForm',
  components: {
    STable,
    AddModal,
    AlterModal
  },
  data () {
    return {
      // 查询参数
      queryParam: {},
      // 表头
      columns: [
        {
          title: '组织',
          dataIndex: 'id'
        },
        {
          title: '上级',
          dataIndex: 'name'
        },
        {
          title: '分类',
          dataIndex: 'createTime',
          sorter: true
        },
        {
          title: '状态',
          dataIndex: 'status',
          sorter: (a, b) => {
            return a.status.localeCompare(b.status)
          },
          align: 'center',
          render: (text) => {
            return text === '启用' ? <Badge color="#52c41a" text={text} /> : <Badge color="#f04134" text={text} />
          }
        },
        {
          title: '操作',
          width: '150px',
          align: 'center',
          dataIndex: 'action',
          scopedSlots: { customRender: 'action' }
        }
      ],
      // 加载数据方法 必须为 Promise 对象
      loadData: parameter => {
        return this.$http
          .get('/role', {
            params: Object.assign(parameter, this.queryParam)
          })
          .then(res => {
            return res.result
          })
      },

      selectedRowKeys: [],
      selectedRows: []
    }
  },
  methods: {
    handleEdit (record) {
      console.log(record)
      this.$refs.modal.edit(record)

      this.mdl = Object.assign({}, record)

      this.mdl.permissions.forEach(permission => {
        permission.actionsOptions = permission.actionEntitySet.map(action => {
          return { label: action.describe, value: action.action, defaultCheck: action.defaultCheck }
        })
      })

      console.log(this.mdl)
      this.visible = true
    },
    handleChange (value, key, column, record) {
      console.log(value, key, column)
      record[column.dataIndex] = value
    },
    edit (row) {
      row.editable = true
      // row = Object.assign({}, row)
    },
    // eslint-disable-next-line

    handleOk () {
      // 新增/修改 成功时，重载列表
      this.$refs.table.refresh()
    },

    // 禁用
    disable (row) {
      this.$confirm({
        title: '禁用成功',
        content: `单位 ${row.id} 确定禁用吗?`,
        okText: '确定',
        okType: 'success',
        // cancelText: '取消',
        onOk () {
          console.log('OK')
          // 在这里调用禁用接口
          return new Promise((resolve, reject) => {
            setTimeout(Math.random() > 0.5 ? resolve : reject, 1000)
          }).catch(() => console.log('Oops errors!'))
        },
        onCancel () {
          console.log('Cancel')
        }
      })
    },

    save (row) {
      row.editable = false
    },
    cancel (row) {
      row.editable = false
    },

    onChange (selectedRowKeys, selectedRows) {
      this.selectedRowKeys = selectedRowKeys
      this.selectedRows = selectedRows
    },
    onSelectChange (selectedRowKeys, selectedRows) {
      this.selectedRowKeys = selectedRowKeys
      this.selectedRows = selectedRows
    },
    toggleAdvanced () {
      this.advanced = !this.advanced
    }
  },
  watch: {
    /*
      'selectedRows': function (selectedRows) {
        this.needTotalList = this.needTotalList.map(item => {
          return {
            ...item,
            total: selectedRows.reduce( (sum, val) => {
              return sum + val[item.dataIndex]
            }, 0)
          }
        })
      }
      */
  }
}
</script>

<style lang="less" scoped>
  .search {
    margin-bottom: 54px;
  }

  .fold {
    width: calc(100% - 216px);
    display: inline-block
  }

  .operator {
    margin-bottom: 18px;
  }

  @media screen and (max-width: 900px) {
    .fold {
      width: 100%;
    }
  }
</style>

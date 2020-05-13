<template>
  <a-card :bordered="false">
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="48">
          <a-col :md="7" :sm="24">
            <a-form-item label="账号">
              <a-input placeholder="请输入"/>
            </a-form-item>
          </a-col>
          <a-col :md="7" :sm="24">
            <a-form-item label="姓名">
              <a-input placeholder="请输入"/>
            </a-form-item>
          </a-col>
          <a-col :md="7" :sm="24">
            <a-form-item label="组织">
              <a-input placeholder="请输入"/>
            </a-form-item>
          </a-col>

          <template v-if="advanced">
            <a-col :md="7" :sm="24">
              <a-form-item label="手机号">
                <a-input placeholder="请输入"/>
              </a-form-item>
            </a-col>
            <a-col :md="7" :sm="24">
              <a-form-item label="记录状态">
                <a-select placeholder="请选择" >
                  <a-select-option value="0">启用</a-select-option>
                  <a-select-option value="1">禁用</a-select-option>
                </a-select>
              </a-form-item>
            </a-col>
          </template>

          <a-col :md="!advanced && 3 || 9" :sm="24">
            <span class="table-page-search-submitButtons" :style="advanced && { float: 'right', overflow: 'hidden' } || {} ">
              <!-- <a-button type="primary">查询</a-button> -->
              <!-- <a-button style="margin-left: 8px">重置</a-button> -->
              <a @click="toggleAdvanced" style="margin-left: 8px">
                {{ advanced ? '收起' : '展开' }}
                <a-icon :type="advanced ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>
          <a-col :md="12" :sm="24" >
            <span>
              <a-button icon="plus" type="primary">
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

    <!-- <div class="table-operator">
      <a-button type="primary" icon="plus">新建</a-button>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1"><a-icon type="delete" />删除</a-menu-item>

          <a-menu-item key="2"><a-icon type="lock" />锁定</a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px">
          批量操作 <a-icon type="down" />
        </a-button>
      </a-dropdown>
    </div> -->

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
          <!-- <span v-else>
            <a class="edit" @click="() => edit(record)">修改11</a>
            <a-divider type="vertical" />
            <a class="delete" @click="() => del(record)">删除</a>
          </span> -->
          <span v-else slot="action" slot-scope="text, record">
            <a class="edit" @click="$refs.modal.edit(record)">修改</a>
            <a-divider type="vertical" />
            <a class="edit" @click="() => disable(record)">禁用</a>
            <a-divider type="vertical" />
            <a-dropdown>
              <a class="ant-dropdown-link">
                更多 <a-icon type="down" />
              </a>
              <a-menu slot="overlay">
                <a-menu-item>
                  <a href="javascript:;">重置密码</a>
                </a-menu-item>
                <a-menu-item>
                  <a href="javascript:;">分配角色</a>
                </a-menu-item>
                <a-menu-item>
                  <a href="javascript:;">查看权限</a>
                </a-menu-item>
              </a-menu>
            </a-dropdown>
          </span>
        </div>
      </template>
    </s-table>

    <role-modal ref="modal" @ok="handleOk"></role-modal>

  </a-card>
</template>

<script>
import { STable } from '@/components'
import RoleModal from './modules/RoleModal'

export default {
  name: 'UserFrom',
  components: {
    STable,
    RoleModal
  },
  data () {
    return {
      // 高级搜索 展开/关闭
      advanced: false,
      // 查询参数
      queryParam: {},
      // 表头
      columns: [
        {
          title: '账号',
          dataIndex: 'no',
          width: '120px'
        },
        {
          title: '名字',
          dataIndex: 'name',
          scopedSlots: { customRender: 'name' }
        },
        {
          title: '服务调用次数',
          dataIndex: 'callNo',
          width: '150px',
          sorter: true,
          needTotal: true,
          scopedSlots: { customRender: 'callNo' }
          // customRender: (text) => text + ' 次'
        },
        {
          title: '更新时间',
          dataIndex: 'updatedAt',
          width: '200px',
          sorter: true,
          scopedSlots: { customRender: 'updatedAt' }
        },
        {
          title: '状态',
          dataIndex: 'status',
          width: '100px',
          needTotal: true,
          scopedSlots: { customRender: 'status' }
        },
        {
          title: '操作',
          align: 'center',
          dataIndex: 'action',
          width: '180px',
          scopedSlots: { customRender: 'action' }
        }
      ],
      // 加载数据方法 必须为 Promise 对象
      loadData: parameter => {
        return this.$http.get('/service', {
          params: Object.assign(parameter, this.queryParam)
        }).then(res => {
          return res.result
        })
      },

      selectedRowKeys: [],
      selectedRows: []
    }
  },
  methods: {
    handleOk () {
      // 新增/修改 成功时，重载列表
      this.$refs.table.refresh()
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
    del (row) {
      this.$confirm({
        title: '警告',
        content: `真的要删除 ${row.no} 吗?`,
        okText: '删除',
        okType: 'danger',
        cancelText: '取消',
        onOk () {
          console.log('OK')
          // 在这里调用删除接口
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

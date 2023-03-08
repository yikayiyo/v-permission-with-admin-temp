<template>
  <div id="app">
    <div class="log">{{ log }}</div>
    <el-table :data="list" style="width: 100%">
      <el-table-column prop="date" label="日期" width="180"> </el-table-column>
      <el-table-column prop="name" label="姓名" width="180"> </el-table-column>
      <el-table-column prop="address" label="地址"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button type="primary">大家都有</el-button>
          <el-button type="success" v-permission="scope.row.ops">Admin Button {{ scope.row.ops }}</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      background
      layout="prev, pager, next"
      :current-page.sync="pageNum"
      :page-size="1"
      :total="tableData.length"
      @current-change="handelPageChange"
    >
    </el-pagination>
  </div>
</template>

<script>
export default {
  name: 'App',
  directives: {
    // 如果没有admin权限，就删除该el
    permission: {
      inserted(el, binding, vnode) {
        const { value } = binding
        const roles = ['admin']
        // console.log(store.getters)
        if (value && value instanceof Array && value.length > 0) {
          const permissionRoles = value

          const hasPermission = roles.some((role) => {
            return permissionRoles.includes(role)
          })

          if (!hasPermission) {
            el.parentNode && el.parentNode.removeChild(el)
          }
        } else {
          throw new Error(`need roles! Like v-permission="['admin','editor']"`)
        }
      },
    },
    focus: {
      // 指令的定义
      inserted: function (el) {
        el.focus()
      },
    },
  },
  methods: {
    handelPageChange(val) {
      this.pageNum = val
      this.list = [this.tableData[val - 1]]
    },
    checkPermission(value) {
      if (value && value instanceof Array && value.length > 0) {
        const roles = ['admin']
        const permissionRoles = value

        const hasPermission = roles.some((role) => {
          return permissionRoles.includes(role)
        })

        if (!hasPermission) {
          return false
        }
        return true
      } else {
        console.error(`need roles! Like v-permission="['admin','editor']"`)
        return false
      }
    },
  },
  data() {
    return {
      activeType: -1,
      log: '',
      pageNum: 1,
      tableData: [
        {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          ops: ['admin'],
        },
        {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄',
          ops: ['test'],
        },
        {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄',
          ops: ['test'],
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄',
          ops: ['admin'],
        },
      ],
      list: [],
    }
  },
  created() {
    this.list = [this.tableData[this.pageNum - 1]]
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

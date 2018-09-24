<template>
  <div>
    <section class="">
      <el-table
        :data="dataSource"
        style="width: 100%">
        <el-table-column
          fixed="left"
          label="操作"
          width="100">
          <template slot-scope="scope">
            <el-button @click="handleRead(scope.row)" type="text" size="small">查看</el-button>
          </template>
        </el-table-column>
        <el-table-column
          prop="id"
          label="id"
          width="180">
        </el-table-column>
        <el-table-column
          prop="title"
          label="标题"
          width="180">
        </el-table-column>
        <el-table-column
          prop="content"
          label="内容">
        </el-table-column>
      </el-table>
      <div class="block">
        <!--<span class="demonstration">博客列表</span>-->
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="$route.query.pageCurrent || pageCurrent"
          :page-size="$route.query.pageSize || pageSize"
          :page-sizes="[10, 20, 50, 100]"
          layout="total, sizes, prev, pager, next, jumper"
          :total="dataSourceTotal">
        </el-pagination>
      </div>
    </section>
  </div>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue'
import axios from 'axios';

export default {
//  watchQuery: ['page'],
  data(){
    return {
      pageCurrent: 1,
      pageSize: 10,
      dataSource: [],
      dataSourceTotal: 0,
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1517 弄'
      }, {
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1519 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄'
      }],
    }
  },
  components: {
    AppLogo
  },
  methods: {
    handleSizeChange(size) {
      console.log(`每页 ${size} 条`);
      this.pageSize = size;
      this.pageCurrent = 1;
//      this.$router.push({ query: {
//        pageCurrent: this.pageCurrent,
//        pageSize: size,
//      } })
      // this.apiBlogs();
      window.location.href = `/blogs/?pageCurrent=${this.pageCurrent}&pageSize=${size}`
    },
    handleCurrentChange(page) {
      this.pageCurrent = page;
      console.log(`当前页: ${page}`);
//      this.$router.push({ query: {
//        pageCurrent: page,
//        pageSize: this.pageSize,
//      } })
      window.location.href = `/blogs/?pageCurrent=${page}&pageSize=${this.pageSize}`
      // this.apiBlogs();
    },
    handleRead(row) {
      console.log('row', row)
      // this.$router.push(`blogs/${row.id}`)
      window.location.href = `/blogs/${row.id}`
    }
  },
  mounted (){},
  asyncData (context, callback) {
    console.log('context', context)
    let pageCurrent = context.route.query.pageCurrent || 1;
    let pageSize = context.route.query.pageSize || 10;
    return axios.get(`http://127.0.0.1:7001/api/blogs?pageCurrent=${pageCurrent}&pageSize=${pageSize}`)
      .then((res) => {
        console.log('res', res.data.data.list)
        callback(null,{
          dataSource: res.data.data.list,
          dataSourceTotal: res.data.data.total,
        })
      })
  }
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>


<template>
  <div class="riskBox">
    <div class="close"><i class="el-icon-close" @click="overTroubleOff"></i></div>
    <div class="riskTitle">逾期隐患详情：{{num}}</div>
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      style="width: 100%;">
      <el-table-column
        prop="troubleNum"
        header-align="center"
        align="center"
        label="隐患编号">
      </el-table-column>
      <el-table-column
        prop="troubleDescription"
        header-align="center"
        align="center"
        label="隐患问题">
      </el-table-column>
      <el-table-column
        prop="troubleType"
        header-align="center"
        align="center"
        label="隐患分级">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.troubleType === 1" size="small">一般隐患</el-tag>
          <el-tag v-if="scope.row.troubleType === 2" size="small">重大隐患</el-tag>
        </template>
      </el-table-column>
      <el-table-column
        prop="measure"
        header-align="center"
        align="center"
        label="管控措施">
      </el-table-column>
      <el-table-column
        prop="department"
        header-align="center"
        align="center"
        label="责任单位">
      </el-table-column>
      <el-table-column
        prop="person"
        header-align="center"
        align="center"
        label="责任人">
      </el-table-column>
      <el-table-column
        prop="checkMan"
        header-align="center"
        align="center"
        label="检查人">
      </el-table-column>
      <el-table-column
        prop="checkType"
        header-align="center"
        align="center"
        label="检查类型">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.checkType === 1" size="small">安全综合检查</el-tag>
          <el-tag v-if="scope.row.checkType === 2" size="small">专业安全检查</el-tag>
          <el-tag v-if="scope.row.checkType === 3" size="small">专项安全检查</el-tag>
          <el-tag v-if="scope.row.checkType === 4" size="small">日常检查</el-tag>
        </template>
      </el-table-column>
      <el-table-column
        prop="checkTime"
        header-align="center"
        align="center"
        label="检查时间">
      </el-table-column>
      <el-table-column
        prop="timeAffirm"
        header-align="center"
        align="center"
        label="确认接收隐患截止时间">
      </el-table-column>
      <el-table-column
        prop="timeAction"
        header-align="center"
        align="center"
        label="责任人整改隐患完成时间">
      </el-table-column>
      <el-table-column
        prop="memo"
        header-align="center"
        align="center"
        label="备注">
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        num: '',
        dataForm: {},
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: []
      }
    },
    methods: {
      activated () {
        this.getDataList()
      },
      // 获取数据列表
      getDataList (num) {
        this.num = num
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/trouble/enterpriseTroublerecord/OverdueTroubleList'),
          method: 'get',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'limit': this.pageSize
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.dataList = data.page.list
            this.totalPage = data.page.totalCount
          } else {
            this.dataList = []
            this.totalPage = 0
          }
          this.dataListLoading = false
        })
      },
      // 每页数
      sizeChangeHandle (val) {
        this.pageSize = val
        this.pageIndex = 1
        this.getDataList(this.num)
      },
      // 当前页
      currentChangeHandle (val) {
        this.pageIndex = val
        this.getDataList(this.num)
      },
      // 关闭dialog
      overTroubleOff () {
        this.$emit('overTroubleHide')
      }
    }
  }
</script>

<style scoped>

</style>

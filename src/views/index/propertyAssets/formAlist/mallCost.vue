<template>
    <section class="form-small-box">
      <el-table
        :data="barrieList"
        border
        style="width: 100%">
        <el-table-column
          prop="dateStart"
          label="开始时间">
        </el-table-column>
        <el-table-column
          prop="dateEnd"
          label="结束时间">
        </el-table-column>
        <el-table-column
          prop="totalCost"
          label="总成本(元)">
        </el-table-column>
        <el-table-column
          label="操作"
          width="70">
          <template scope="scope">
            <router-link class="el-icon-document"
                          target="_blank"
                          :to="{
                            name: 'form-mall',
                            query: {
                              mallCode: scope.row.mallCode,
                              type: 'mallCost',
                              id: scope.row.id
                            }
                          }">
            </router-link>
          </template>
        </el-table-column>
      </el-table>
      <div class="clear"></div>
      <el-pagination
          v-if="total"
          class="page-box"
          @current-change="pageChange"
          layout="prev, pager, next"
          :page-size="pageSize"
          :total="total">
      </el-pagination>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import { mapGetters } from 'vuex'
export default {
    props: ['base'],
    data () {
        return {
            barrieList: [],
            pageNumber: 1,
            pageSize: 5,
            total: 0
        }
    },
    mounted () {
      this.getList()
    },
    computed: {
        ...mapGetters({
            userInfo: 'getUserInfo'
        }),
        isEdit () {
          return this.base.recoder == this.userInfo.userCode
        }
    },
    methods: {
        getList () {
          util.request({
              method: 'get',
              interface: 'mallCostList',
              data: {
                mallCode: this.$route.query.mallCode,
                pageNumber: this.pageNumber,
                pageSize: this.pageSize
              }
          }).then(res => {
              if (res.result.success == '1') {
                this.total = Number(res.result.total)
                this.barrieList = res.result.result
              } else {
                this.$message.error(res.result.message)
              }
          })
        },
        pageChange (size) {
            this.pageNumber = size
            this.getList()
        }
    }
}
</script>
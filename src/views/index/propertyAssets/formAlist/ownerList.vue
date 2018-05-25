<template>
    <section class="activity-box">
        <el-table
          :data="itemList"
          border
          style="width: 100%">
          <el-table-column
            prop="ownerStockRight"
            label="股权比例(%)">
          </el-table-column>
          <el-table-column
            label="业主名称">
            <template scope="scope">
              <router-link class="black nav-hover"
                            target="_blank"
                            :to="{
                              name: 'enterprise-info',
                              query: {
                                enterpriseCode: scope.row.ownerEnterpriseCode
                              }
                            }">
                  {{scope.row.enterpriseNameReg}}
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
import searchInput from '../../../../components/common/search-input'
import { mapGetters } from 'vuex'

export default {
    props: ['base', 'enterpriseList', 'propertyCode', 'propertyType'],
    data () {
        return {
          operateText: '添加',
          itemList: [],
          isAddOEdit: false,
          itemData: {
            propertyCode: '',
            ownerStockRight: '',
            propertyType: '',
            ownerEnterpriseCode: ''
          },
          itemNowIndex: '',
          pageNumber: 1,
          pageSize: 10,
          total: 0
        }
    },
    mounted () {
      this.getItemList()
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
      getEnterpriseList (queryString, cb) {
        if (!this.requestNum) {
          this.requestNum = 1
        } else {
          this.requestNum++
        }
        
        var requestNum = this.requestNum

        var formData = {
          keyName: queryString,
          status: '0',
          pageNumber: 1,
          pageSize: 20
        }

        util.request({
            method: 'get',
            interface: 'enterpriseListByKeyname',
            data: formData
        }).then(res => {
            if (res.result.success == '1') {
                if (requestNum == this.requestNum) {
                  if (res.result.result.length) {
                    cb(res.result.result)
                  }
                }
            } else {
                this.$message.error(res.result.message)
            }
        })       
      },
      getItemList () {
        util.request({
            method: 'get',
            interface: 'propertyOwnerList',
            data: {
              propertyCode: this.propertyCode,
              pageNumber: this.pageNumber,
              pageSize: this.pageSize
            }
        }).then(res => {
            if (res.result.success == '1') {
                this.total = Number(res.result.total)
                this.itemList = res.result.result
            } else {
                this.$message.error(res.result.message)
            }  
        })
      },
      pageChange (size) {
          this.pageNumber = size
          this.getItemList()
      }
    },
    components: {
      searchInput
    }
}
</script>
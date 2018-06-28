<template>
    <section class="activity-box">
        <div class="add-btn">
          <el-button type="primary" icon="plus" size="small"
                  @click="addItem"
                  v-if="isEdit">增加</el-button>
        </div>
        
        <el-table
          :data="itemList"
          border
          style="width: 100%">
          <el-table-column
            prop="assetRatio"
            label="贡献比例(%)">
          </el-table-column>
          <el-table-column
            prop="propertyName"
            label="物业名称">
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
    props: ['base', 'enterpriseList', 'securityCode', 'securityType', 'dictionary'],
    data () {
        return {
          operateText: '添加',
          itemList: [],
          propertyList: [],
          nameKey: '',
          codeKey: '',
          isAddOEdit: false,
          itemData: {
            securityCode: '',
            assetRatio: '',
            securityType: '',
            propertyType: '',
            propertyCode: '',
            propertyName: ''
          },
          pageNumber: 1,
          pageSize: 5,
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
      getPropertList (queryString, cb) {
        if (!this.requestNum) {
          this.requestNum = 1
        } else {
          this.requestNum++
        }
        
        var requestNum = this.requestNum
        var interfaceName = ''

        console.log(this.itemData.propertyType)

        switch (this.itemData.propertyType) {
          case 'property_type_1':
            interfaceName = 'officeList'
            this.nameKey = 'officeName'
            this.codeKey = 'officeCode'
            break
          case 'property_type_2':
            interfaceName = 'mallInfoList'
            this.nameKey = 'mallName'
            this.codeKey = 'mallCode'
            break
        }

        var formData = {
          enterpriseCode: this.$route.query.enterpriseCode,
          pageNumber: 1,
          pageSize: 20
        }

        formData[this.nameKey] = queryString

        util.request({
            method: 'get',
            interface: interfaceName,
            data: formData
        }).then(res => {
            if (res.result.success == '1') {
                if (requestNum == this.requestNum) {
                  cb(res.result.result)
                }
            } else {
                this.$message.error(res.result.message)
            }
        })       
      },
      getItemList () {
        util.request({
            method: 'get',
            interface: 'securityAssetList',
            data: {
              securityCode: this.securityCode,
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
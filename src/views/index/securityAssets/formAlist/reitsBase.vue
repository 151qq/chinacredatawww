<template>
    <section>
      <div class="formDiscount static-box">
          <section class="formBox">
              <span>产品名称</span>
              <div class="input-box">
                {{base.foreignReitsName}}
              </div>
          </section>
          <section class="formBox">
              <span>产品编码</span>
              <div class="input-box">
                {{base.foreignReitsId}}
              </div>
          </section>
          <section class="formBox">
              <span>上市时间</span>
              <div class="input-box">
                {{base.listedDate.length ? base.listedDate.split(' ')[0] : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>成立时间</span>
              <div class="input-box">
                {{base.createdDate.length ? base.createdDate.split(' ')[0] : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>基金管理人</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'fundManager'"
                              :m-name="'fundManagerName'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox">
              <span>发行价格(万元)</span>
              <div class="input-box">
                {{base.issuePrice}}
              </div>
          </section>
          <section class="formBox">
              <span>发行股份数</span>
              <div class="input-box">
                {{base.fundShares}}
              </div>
          </section>
          <section class="formBox">
              <span>相关链接</span>
              <div class="input-box">
                {{base.foreignReitsWeb}}
              </div>
          </section>
          <section class="formBox">
              <span>主承销商</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'mainUnderwriter'"
                              :m-name="'mainUnderwriterName'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox">
              <span>评级机构一</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'evaluationAgency1'"
                              :m-name="'evaluationAgency1Name'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox">
              <span>评级机构二</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'evaluationAgency2'"
                              :m-name="'evaluationAgency2Name'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox">
              <span>评级机构三</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'evaluationAgency3'"
                              :m-name="'evaluationAgency3Name'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox">
              <span>评级机构四</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'evaluationAgency4'"
                              :m-name="'evaluationAgency4Name'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox">
              <span>评级机构五</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'evaluationAgency5'"
                              :m-name="'evaluationAgency5Name'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox">
              <span>物业持有人</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'propertyFundOwner'"
                              :m-name="'propertyFundOwnerName'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox bigF">
              <span>产品描述</span>
              <div class="input-box">
                {{base.foreignReitsDesc}}
              </div>
          </section>
          <section class="formBox">
            <span>产品封面</span>
            <div class="static-img">
                <img :src="base.foreignReitsCover">
            </div>
          </section>
          <div class="clear"></div>
      </div>
    </section>
    
</template>
<script>
import util from '../../../../assets/common/util'
import searchInput from '../../../../components/common/search-input'
import { mapGetters } from 'vuex'
export default {
    props: ['enterpriseList', 'dictionary'],
    data () {
        return {
            base: {
              foreignReitsDesc: ''
            }
        }
    },
    mounted () {
      this.getBase()
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
        changeImgCover () {
          this.base.foreignReitsCover = data.url
        },
        getBase () {
          util.request({
              method: 'get',
              interface: 'foreignReitsInfo',
              data: {
                foreignReitsCode: this.$route.query.foreignReitsCode
              }
          }).then(res => {
              if (res.result.success == '1') {
                this.base = res.result.result
                this.$emit('change', res.result.result)
              } else {
                this.$message.error(res.result.message)
              }
          })
        }
    },
    components: {
      searchInput
    }
}
</script>
<style lang="scss">
.product-base-box {
  overflow: hidden;

  .form-box {
    overflow: hidden;
  }
}
</style>
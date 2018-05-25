<template>
    <section class="float-form-box">
      <div class="formDiscount static-box">
          <section class="formBox">
              <span>交易时间</span>
              <div class="input-box">
                {{barrieData.tradeDate.length ? barrieData.tradeDate.split(' ')[0] : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>交易甲方</span>
              <search-input   :class="'input-box'"
                              :search-data="barrieData"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'firstParty'"
                              :m-name="'firstPartyName'"
                              :m-type="'firstPartyType'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"
                              :s-type="'enterpriseType'"></search-input>
          </section>
          <section class="formBox">
              <span>交易乙方</span>
              <search-input   :class="'input-box'"
                              :search-data="barrieData"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'secondParty'"
                              :m-name="'secondPartyName'"
                              :m-type="'secondPartyType'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"
                              :s-type="'enterpriseType'"></search-input>
          </section>
          <section class="formBox">
              <span>交易类型</span>
              <div class="input-box">
                {{barrieData.tradeType ? dictionary[barrieData.tradeType] : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>股权比例(%)</span>
              <div class="input-box">
                {{barrieData.stockRightRation}}
              </div>
          </section>
          <section class="formBox">
              <span>股权交易场所</span>
              <div class="input-box">
                {{barrieData.stockTracdePlace}}
              </div>
          </section>
          <section class="formBox">
              <span>交易价格</span>
              <div class="input-box">
                {{barrieData.tradePrice}}
              </div>
          </section>
          <section class="formBox bigF">
            <span>交易说明</span>
            <div class="input-box">
                {{barrieData.tradeDesc}}
            </div>
          </section>      
        </div>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import searchInput from '../../../../components/common/search-input'
import { mapGetters } from 'vuex'
export default {
    props: ['base', 'propertyCode', 'propertyType', 'dictionary'],
    data () {
        return {
            barrieData: {
              propertyCode: '',
              propertyType: '',
              tradeDate: '',
              firstParty: '',
              firstPartyType: '',
              secondParty: '',
              secondPartyType: '',
              tradeType: '',
              stockRightRation: '',
              stockTracdePlace: '',
              tradePrice: '',
              recoder: '',
              tradeDesc: ''
            }
        }
    },
    mounted () {
      this.getInfo()
    },
    watch: {
      $route () {
        this.getInfo()
      }
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
        getInfo () {
          if (!this.$route.query.id) {
            this.barrieData.propertyCode = this.propertyCode
            this.barrieData.propertyType = this.propertyType
            return false
          }

          util.request({
              method: 'get',
              interface: 'propertyTradeInfo',
              data: {
                propertyCode: this.propertyCode,
                id: this.$route.query.id
              }
          }).then(res => {
              if (res.result.success == '1') {
                this.barrieData = res.result.result
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
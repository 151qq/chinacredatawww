<template>
    <section class="float-form-box">
        <div class="formDiscount static-box">
          <section class="formBox">
              <span>名称</span>
              <div class="input-box">
                {{barrieData.fundLevelName}}
              </div>
          </section>
          <section class="formBox">
              <span>比例(%)</span>
              <div class="input-box">
                {{barrieData.fundLevelRatio}}
              </div>
          </section>
          <section class="formBox">
              <span>编码</span>
              <div class="input-box">
                {{barrieData.fundLevelStockId}}
              </div>
          </section>
          <section class="formBox">
              <span>发行规模(万元)</span>
              <div class="input-box">
                {{barrieData.fundLevelMoney}}
              </div>
          </section>
          <section class="formBox">
              <span>最低利率</span>
              <div class="input-box">
                {{barrieData.fundLevelInterestMinRate}}
              </div>
          </section>
          <section class="formBox">
              <span>最高利率</span>
              <div class="input-box">
                {{barrieData.fundLevelInterestMaxRate}}
              </div>
          </section>
          <section class="formBox">
              <span>类型</span>
              <div class="input-box">
                {{barrieData.fundLevelType ? dictionary[barrieData.fundLevelType] : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>还本方式</span>
              <div class="input-box">
                {{barrieData.fundLevelRepaymentType ? dictionary[barrieData.fundLevelRepaymentType] : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>期限</span>
              <div class="input-box">
                {{barrieData.fundLevelRepaymentDuration}}
              </div>
          </section>
          <section class="formBox">
              <span>评级</span>
              <div class="input-box">
                {{barrieData.fundLevelRating ? dictionary[barrieData.fundLevelRating] : ''}}
              </div>
          </section>
          <section class="formBox bigF">
            <span>备注</span>
            <div class="input-box">
                {{barrieData.fundLevelDesc}}
            </div>
          </section> 
        </div>
      </section>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import { mapGetters } from 'vuex'
export default {
    props: ['base', 'dictionary'],
    data () {
        return {
            barrieData: {
              assetManagementProductCode: '',
              fundLevelName: '',
              fundLevelRatio: '',
              fundLevelStockId: '',
              fundLevelMoney: '',
              fundLevelInterestMinRate: '',
              fundLevelInterestMaxRate: '',
              fundLevelType: '',
              fundLevelRepaymentType: '',
              fundLevelRepaymentDuration: '',
              fundLevelRating: '',
              fundLevelDesc: ''
            },
            pageNumber: 1,
            pageSize: 3,
            total: 0
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
        getInfo () {
          if (!this.$route.query.id) {
            this.barrieData.assetManagementProductCode = this.$route.query.assetManagementProductCode
            return false
          }

          util.request({
              method: 'get',
              interface: 'assetManagementProductLevelInfo',
              data: {
                assetManagementProductCode: this.$route.query.assetManagementProductCode,
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
    }
}
</script>
<template>
    <section class="float-form-box">
        <div class="formDiscount static-box">
          <section class="formBox">
              <span>时间</span>
              <div class="input-box">
                {{barrieData.evaluationDate.length ? barrieData.evaluationDate : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>总收入</span>
              <div class="input-box">
                {{barrieData.totalIncome}}
              </div>
          </section>
          <section class="formBox">
              <span>总成本(元)</span>
              <div class="input-box">
                {{barrieData.totalCost}}
              </div>
          </section>
          <section class="formBox">
              <span>息税前利润(元)</span>
              <div class="input-box">
                {{barrieData.totalProfit}}
              </div>
          </section>
          <section class="formBox">
              <span>折旧与摊销(元)</span>
              <div class="input-box">
                {{barrieData.depreciationAmortization}}
              </div>
          </section>
          <section class="formBox">
              <span>财务支出(元)</span>
              <div class="input-box">
                {{barrieData.financeCost}}
              </div>
          </section>
          <section class="formBox bigF">
            <span>财务支出说明</span>
            <div class="input-box">
                {{barrieData.financeCostDesc}}
            </div>
          </section>
          <section class="formBox">
              <span>所得税(元)</span>
              <div class="input-box">
                {{barrieData.incomeTax}}
              </div>
          </section>
          <section class="formBox">
              <span>净利润(元)</span>
              <div class="input-box">
                {{barrieData.netProfit}}
              </div>
          </section>
          <section class="formBox">
              <span>资本化率(%)</span>
              <div class="input-box">
                {{barrieData.capitalizaitonRate}}
              </div>
          </section>
          <section class="formBox">
              <span>估值(元)</span>
              <div class="input-box">
                {{barrieData.propertyValue}}
              </div>
          </section>
          <section class="formBox">
              <span>估值人</span>
              <div class="input-box">
                {{barrieData.propertyValuer}}
              </div>
          </section>
          <section class="formBox bigF">
            <span>估值说明</span>
            <div class="input-box">
                {{barrieData.propertyEvaluationDesc}}
            </div>
          </section> 
        </div>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import { mapGetters } from 'vuex'
export default {
    props: ['base', 'propertyCode', 'propertyType'],
    data () {
        return {
            barrieData: {
              propertyCode: '',
              propertyType: '',
              evaluationDate: '',
              totalIncome: '',
              totalCost: '',
              totalProfit: '',
              depreciationAmortization: '',
              financeCost: '',
              financeCostDesc: '',
              incomeTax: '',
              netProfit: '',
              capitalizaitonRate: '',
              propertyValue: '',
              propertyValuer: '',
              propertyEvaluationDesc: ''
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
        getInfo () {
          if (!this.$route.query.id) {
            this.barrieData.propertyCode = this.propertyCode
            this.barrieData.propertyType = this.propertyType
            return false
          }

          util.request({
              method: 'get',
              interface: 'propertyValueInfo',
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
    }
}
</script>
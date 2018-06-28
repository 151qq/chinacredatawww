<template>
    <section class="float-form-box">
        <div class="formDiscount static-box">
          <section class="formBox">
            <span>开始时间</span>
            <div class="input-box">
              {{barrieData.dateStart.length ? barrieData.dateStart : ''}}
            </div>
          </section>
          <section class="formBox">
            <span>结束时间</span>
            <div class="input-box">
              {{barrieData.dateEnd.length ? barrieData.dateEnd : ''}}
            </div>
          </section>
          <section class="formBox">
              <span>年化租金(元)</span>
              <div class="input-box">
                {{barrieData.annualRentIncome}}
              </div>
          </section>
          <section class="formBox">
              <span>能源费(元)</span>
              <div class="input-box">
                {{barrieData.energyIncome}}
              </div>
          </section>
          <section class="formBox">
              <span>物业管理费(元)</span>
              <div class="input-box">
                {{barrieData.estateManagementIncome}}
              </div>
          </section>
          <section class="formBox">
              <span>广告费(元)</span>
              <div class="input-box">
                {{barrieData.adIncome}}
              </div>
          </section>
          <section class="formBox">
              <span>车位费(元)</span>
              <div class="input-box">
                {{barrieData.packingLotIncome}}
              </div>
          </section>
          <section class="formBox">
              <span>花车费(元)</span>
              <div class="input-box">
                {{barrieData.flowerIncome}}
              </div>
          </section>
          <section class="formBox">
              <span>其他收入(元)</span>
              <div class="input-box">
                {{barrieData.otherIncome}}
              </div>
          </section>
          <section class="formBox">
              <span>总收入(元)</span>
              <div class="input-box">
                {{barrieData.totalIncome}}
              </div>
              
          </section>
          <section class="formBox">
            <span>其他收入说明</span>
            <div class="input-box">
                {{barrieData.otherIncomeDesc}}
            </div>
          </section>
          <section class="formBox bigF">
            <span>备注</span>
            <div class="input-box">
                {{barrieData.memo}}
            </div>
          </section> 
        </div>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import { mapGetters } from 'vuex'
export default {
    props: ['base'],
    data () {
        return {
            barrieData: {
              mallCode: '',
              dateType: '2',
              dateStart: '',
              dateEnd: '',
              annualRentIncome: '',
              energyIncome: '',
              estateManagementIncome: '',
              adIncome: '',
              packingLotIncome: '',
              flowerIncome: '',
              otherIncome: '',
              totalIncome: '',
              otherIncomeDesc: '',
              memo: ''
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
            this.barrieData.officeCode = this.$route.query.officeCode
            return false
          }

          util.request({
              method: 'get',
              interface: 'mallIncomeInfo',
              data: {
                mallCode: this.$route.query.mallCode,
                id: this.$route.query.id
              }
          }).then(res => {
              if (res.result.success == '1') {
                res.result.result.yearDate = new Date(res.result.result.dateYear)
                this.barrieData = res.result.result
              } else {
                this.$message.error(res.result.message)
              }
          })
        }
    }
}
</script>
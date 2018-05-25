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
              <span>运维成本</span>
              <div class="input-box">
                {{barrieData.maintainCost}}
              </div>
          </section>
          <section class="formBox">
              <span>人力成本</span>
              <div class="input-box">
                {{barrieData.hrCost}}
              </div>
          </section>
          <section class="formBox">
              <span>管理成本</span>
              <div class="input-box">
                {{barrieData.managementCost}}
              </div>
          </section>
          <section class="formBox">
              <span>其他成本</span>
              <div class="input-box">
                {{barrieData.otherCost}}
              </div>
          </section>
          <section class="formBox">
              <span>税费</span>
              <div class="input-box">
                {{barrieData.tax}}
              </div>
          </section>
          <section class="formBox">
              <span>总成本</span>
              <div class="input-box">
                {{barrieData.totalCost}}
              </div>
          </section>
          <section class="formBox bigF">
            <span>其他成本说明</span>
            <div class="input-box">
                {{barrieData.otherCostDesc}}
            </div>
          </section>
          <section class="formBox bigF">
            <span>税费说明</span>
            <div class="input-box">
                {{barrieData.taxDesc}}
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
              officeCode: '',
              dateType: '2',
              dateStart: '',
              dateEnd: '',
              maintainCost: '',
              hrCost: '',
              managementCost: '',
              otherCost: '',
              otherCostDesc: '',
              tax: '',
              totalCost: '',
              taxDesc: '',
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
              interface: 'officeCostInfo',
              data: {
                officeCode: this.$route.query.officeCode,
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
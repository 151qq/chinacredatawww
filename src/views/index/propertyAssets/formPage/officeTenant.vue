<template>
    <section class="float-form-box"> 
      <div class="formDiscount static-box">
          <section class="formBox bigBigSpan">
              <span>开始时间</span>
              <div class="input-box">
                {{barrieData.beginDate.length ? barrieData.beginDate : ''}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>结束时间</span>
              <div class="input-box">
                {{barrieData.endDate.length ? barrieData.endDate : ''}}
              </div>
          </section>
        <section class="formBox">
            <span>租户名称</span>
            <div class="input-box">
              {{barrieData.tenantName}}
            </div>
        </section>
        <section class="formBox">
            <span>租户类型</span>
            <div class="input-box">
              {{barrieData.tenantType ? dictionary[barrieData.tenantType] : ''}}
            </div>
        </section>
        <section class="formBox">
            <span>免租期</span>
            <div class="input-box">
              {{barrieData.rentFreePeriod}}
            </div>
        </section>
        <section class="formBox">
            <span>面积(m²)</span>
            <div class="input-box">
              {{barrieData.rentAcreage}}
            </div>
        </section>
        <section class="formBox bigF">
          <span>租用目的</span>
          <div class="input-box">
              {{barrieData.rentBusinessGoal}}
          </div>
        </section>
        <section class="formBox bigF">
          <span>租户简介</span>
          <div class="input-box">
              {{barrieData.tenantDesc}}
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
    props: ['base', 'dictionary'],
    data () {
        return {
            barrieData: {
              officeCode: '',
              tenantName: '',
              tenantType: '',
              rentFreePeriod: '',
              rentAcreage: '',
              tenantDesc: '',
              rentBusinessGoal: '',
              memo: '',
              beginDate: '',
              endDate: ''
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
              interface: 'officeTenantInfoInfo',
              data: {
                officeCode: this.$route.query.officeCode,
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
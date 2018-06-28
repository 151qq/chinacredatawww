<template>
    <section class="float-form-box">
        <div class="formDiscount static-box">
          <section class="formBox">
              <span>开始时间</span>
              <div class="input-box">
                {{barrieData.beginDate.length ? barrieData.beginDate.split(' ')[0] : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>结束时间</span>
              <div class="input-box">
                {{barrieData.endDate.length ? barrieData.endDate.split(' ')[0] : ''}}
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
              <span>租金类型</span>
              <div class="input-box">
                {{barrieData.leaseType ? dictionary[barrieData.leaseType] : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>免租期</span>
              <div class="input-box">
                {{barrieData.rentFreePeriod}}
              </div>
          </section>
          <section class="formBox">
              <span>主力店</span>
              <div class="input-box">
                {{barrieData.tenantLevelName}}
              </div>
          </section>
          <section class="formBox">
              <span>装修补贴</span>
              <div class="input-box">
                {{barrieData.constructionAllowance}}
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
              mallCode: '',
              rangeDate: [],
              tenantName: '',
              tenantType: '',
              leaseType: '',
              tenantLevel: '',
              rentFreePeriod: '',
              constructionAllowance: '',
              rentAcreage: '',
              tenantDesc: '',
              rentBusinessGoal: '',
              memo: '',
              beginDate: '',
              endDate: ''
            },
            tenantLevels: [
              {
                typeName: '主力店',
                id: '1'
              },
              {
                typeName: '非主力店',
                id: '0'
              }
            ]
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
            this.barrieData.mallCode = this.$route.query.mallCode
            return false
          }

          util.request({
              method: 'get',
              interface: 'mallTenantInfoInfo',
              data: {
                mallCode: this.$route.query.mallCode,
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
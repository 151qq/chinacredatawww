<template>
    <section class="float-form-box">
        <div class="formDiscount static-box">
          <section class="formBox bigBigSpan">
              <span>开始时间</span>
              <div class="input-box">
                {{barrieData.beginDate.length ? barrieData.beginDate.split(' ')[0] : ''}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>结束时间</span>
              <div class="input-box">
                {{barrieData.endDate.length ? barrieData.endDate.split(' ')[0] : ''}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>高区租金(元 ㎡/天)</span>
              <div class="input-box">
                {{barrieData.rentHighLevel}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>中区租金(元 ㎡/天)</span>
              <div class="input-box">
                {{barrieData.rentMiddleLevel}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>低区租金(元 ㎡/天)</span>
              <div class="input-box">
                {{barrieData.rentLowLevel}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>地上商业租金(元 ㎡/天)</span>
              <div class="input-box">
                {{barrieData.rentBusiness}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>地下商业租金(元 ㎡/天)</span>
              <div class="input-box">
                {{barrieData.rentUndergroundBusiness}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>高区空置率(%)</span>
              <div class="input-box">
                {{barrieData.vacancyHighLevel}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>中区空置率(%)</span>
              <div class="input-box">
                {{barrieData.vacancyMiddleLevel}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>低区空置率(%)</span>
              <div class="input-box">
                {{barrieData.vacancyLowLevel}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>地下商业空置率(%)</span>
              <div class="input-box">
                {{barrieData.vacancyUndergroundBusiness}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>地上商业空置率(%)</span>
              <div class="input-box">
                {{barrieData.vacancyBusiness}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>平均空置率(%)</span>
              <div class="input-box">
                {{barrieData.averageVacancy}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>时租停车费(元)</span>
              <div class="input-box">
                {{barrieData.parkingHourFee}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>月租停车费(元)</span>
              <div class="input-box">
                {{barrieData.parkingMonthFee}}
              </div>
          </section>
          <section class="formBox bigBigSpan bigBigSpanF">
            <span>停车费说明</span>
            <div class="input-box">
              {{barrieData.parkingFeeDesc}}
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
              beginDate: '',
              endDate: '',
              rentHighLevel: '',
              rentMiddleLevel: '',
              rentLowLevel: '',
              rentUndergroundBusiness: '',
              rentBusiness: '',
              vacancyHighLevel: '',
              vacancyMiddleLevel: '',
              vacancyLowLevel: '',
              vacancyUndergroundBusiness: '',
              vacancyBusiness: '',
              averageVacancy: '',
              parkingHourFee: '',
              parkingMonthFee: '',
              parkingFeeDesc: ''
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
              interface: 'officeRentHistoryInfo',
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
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
              <span>楼层</span>
              <div class="input-box">
                {{barrieData.floorLevel}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>楼层位置</span>
              <div class="input-box">
                {{barrieData.floorZone}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>平均租金(元 ㎡/天)</span>
              <div class="input-box">
                {{barrieData.rentAve}}
              </div>
          </section>
          <section class="formBox bigBigSpan">
              <span>空置率(%)</span>
              <div class="input-box">
                {{barrieData.vacancyAve}}
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
              mallCode: '',
              beginDate: '',
              endDate: '',
              floorLevel: '',
              floorZone: '',
              rentAve: '',
              vacancyAve: '',
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
            this.barrieData.mallCode = this.$route.query.mallCode
            return false
          }

          util.request({
              method: 'get',
              interface: 'mallRentHistoryInfo',
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
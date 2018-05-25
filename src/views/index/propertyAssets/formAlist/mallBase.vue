<template>
    <section>
      <div class="formDiscount static-box">
          <section class="formBox">
              <span>购物中心类型</span>
              <div class="input-box">
                {{base.mallType ? dictionary[base.mallType] : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>地上建筑面积(m²)</span>
              <div class="input-box">
                {{base.groundBuiltupArea}}
              </div>
          </section>
          <section class="formBox">
              <span>地下建筑面积(m²)</span>
              <div class="input-box">
                {{base.undergroundBuildupArea}}
              </div>
          </section>
          <section class="formBox">
              <span>总建筑面积(m²)</span>
              <div class="input-box">
                {{totalArea}}
              </div>
          </section>
          <section class="formBox">
              <span>地上经营面积(m²)</span>
              <div class="input-box">
                {{base.groundOperationAera}}
              </div>
          </section>
          <section class="formBox">
              <span>地下经营面积(m²)</span>
              <div class="input-box">
                {{base.undergroundOperationArea}}
              </div>
          </section>
          <section class="formBox">
              <span>地上可租面积(m²)</span>
              <div class="input-box">
                {{base.groundRentArea}}
              </div>
          </section>
          <section class="formBox">
              <span>地下可租面积(m²)</span>
              <div class="input-box">
                {{base.undergroundRentArea}}
              </div>
          </section>
          <section class="formBox">
              <span>层高(m)</span>
              <div class="input-box">
                {{base.floorHeight}}
              </div>
          </section>
          <section class="formBox">
              <span>净高(m)</span>
              <div class="input-box">
                {{base.floorNetHeight}}
              </div>
          </section>
          <section class="formBox">
              <span>容积率(%)</span>
              <div class="input-box">
                {{base.plotRation}}
              </div>
          </section>
          <section class="formBox">
              <span>车位数量</span>
              <div class="input-box">
                {{base.parkingNumber}}
              </div>
          </section>
          <section class="formBox">
              <span>地上层数</span>
              <div class="input-box">
                {{base.floorOvergroundLayer}}
              </div>
          </section>
          <section class="formBox">
              <span>地下层数</span>
              <div class="input-box">
                {{base.floorUndergroundLayer}}
              </div>
          </section>
          <section class="formBox">
              <span>电梯数量</span>
              <div class="input-box">
                {{base.liftNumber}}
              </div>
          </section>
          <section class="formBox">
              <span>空调类型</span>
              <div class="input-box">
                {{base.airconditionerType ? dictionary[base.airconditionerType] : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>地板类型</span>
              <div class="input-box">
                {{base.floorType ? dictionary[base.floorType] : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>设计单位</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'designerUnit'"
                              :m-name="'designerUnitName'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox">
              <span>施工单位</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'builderUnit'"
                              :m-name="'builderUnitName'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox">
              <span>物业公司</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'propertyManagementUnit'"
                              :m-name="'propertyManagementUnitName'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox">
              <span>商业运营单位</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'mallBusinessOperator'"
                              :m-name="'mallBusinessOperatorName'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox">
              <span>购物中心官网</span>
              <div class="input-box">
                {{base.mallWeb}}
              </div>
          </section>
          <section class="formBox">
              <span>物业持有</span>
              <div class="input-box">
                {{base.propertySaleModelName}}
              </div>
          </section>
          <section class="formBox">
              <span>地类(用途)</span>
              <div class="input-box">
                {{base.landTypeName}}
              </div>
          </section>
          <section class="formBox">
              <span>使用权类型</span>
              <div class="input-box">
                {{base.landUsageRightTypeName}}
              </div>
          </section>
          <section class="formBox">
              <span>土地使用权所属</span>
              <search-input class="input-box"
                              :search-data="base"
                              :fetch-suggestions="getEnterpriseList"
                              :m-code="'landUsageUnit'"
                              :m-name="'landUsageUnitName'"
                              :s-code="'enterpriseCode'"
                              :s-name="'enterpriseNameReg'"></search-input>
          </section>
          <section class="formBox">
              <span>获地时间</span>
              <div class="input-box">
                {{base.landGetDate.length ? base.landGetDate.split(' ')[0] : ''}}
              </div>
          </section>
          <section class="formBox bigF">
              <span>土地使用说明</span>
              <div class="input-box">
                {{base.landOtherMemo}}
              </div>
          </section>
          <section class="formBox">
              <span>土地剩余年限</span>
              <div class="input-box">
                {{base.landLifeLeft}}
              </div>
          </section>
          <section class="formBox">
              <span>投入使用时间</span>
              <div class="input-box">
                {{base.mallOpenDate.length ? base.mallOpenDate.split(' ')[0] : ''}}
              </div>
          </section>
          <section class="formBox">
              <span>物业管理经理</span>
              <div class="input-box">
                {{base.propertyManager}}
              </div>
          </section>
          <section class="formBox bigF">
              <span>购物中心介绍</span>
              <div class="input-box">
                {{base.propertyDesc}}
              </div>
          </section>
          <div class="clear"></div>
      </div>
    </section>
    
</template>
<script>
import util from '../../../../assets/common/util'
import searchInput from '../../../../components/common/search-input'
import searchBox from '../../../../components/common/search-box.vue'
import mapShow from '../../../../components/common/map-show.vue'
import { mapGetters } from 'vuex'
export default {
    props: ['enterpriseList', 'dictionary'],
    data () {
        return {
            base: {
              mallCode: '',
              mallAddr: '',
              mallName: '',
              mallGps: '',
              zoneCode: '',
              mallType: '',
              mallCover: '',
              groundBuiltupArea: '',
              undergroundBuildupArea: '',
              groundOperationAera: '',
              undergroundOperationArea: '',
              groundRentArea: '',
              undergroundRentArea: '',
              plotRation: '',
              floorHeight: '',
              floorNetHeight: '',
              floorOvergroundLayer: '',
              floorUndergroundLayer: '',
              parkingNumber: '',
              floorType: '',
              airconditionerType: '',
              liftNumber: '',
              designerUnit: '',
              builderUnit: '',
              propertyManagementUnit: '',
              mallBusinessOperator: '',
              mallWeb: '',
              propertySaleModel: '',
              landType: '',
              landUsageRightType: '',
              landUsageUnit: '',
              landGetDate: '',
              landLifeLeft: '',
              mallOpenDate: '',
              landOtherMemo: '',
              propertyManager: '',
              propertyDesc: '',
              propertyAlbum: '',
              recoder: ''
            },
            totalArea: ''
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
        mapChange (mapInfo) {
          this.base.mallAddr = mapInfo.title
          this.base.mallGps = mapInfo.point.lng + ',' + mapInfo.point.lat
        },
        getBase () {
          util.request({
              method: 'get',
              interface: 'mallInfo',
              data: {
                mallCode: this.$route.query.mallCode
              }
          }).then(res => {
              if (res.result.success == '1') {
                this.base = res.result.result
                this.totalArea = Number(this.base.groundBuiltupArea) + Number(this.base.undergroundBuildupArea)
                this.$emit('change', res.result.result)
              } else {
                this.$message.error(res.result.message)
              }
          })
        }
    },
    components: {
      searchInput,
      searchBox,
      mapShow
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
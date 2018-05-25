<template>
    <div class="card-list-box">
        <div class="input-box">
            <input
                placeholder="请输入需查询条件"
                v-model="keyValue"
                @keyup.13="searchItem"
                class="input-search">
            <el-button class="search-btn" type="primary" icon="search" @click="searchItem">
              搜索
            </el-button>
        </div>
        
        <!-- 列表 -->
        <div class="clear"></div>
        <template v-if="sourceDatas.length">
            <section class="check-box" v-for="(item, index) in sourceDatas" :key="index">
                <!-- 选择框 -->
                <router-link class="sou-box"
                            target="_blank"
                            :to="{
                                name: 'assets-list',
                                query: {
                                    enterpriseCode: userInfo.enterpriseCode,
                                    zoneCode: item.zoneCode,
                                    cityName: item.cityName,
                                    cityCode: item.cityCode
                                }
                            }">
                    <div class="cover-box">
                        <img :src="item.zoneCover">
                    </div>
                    <div class="title-box">
                        <div class="title">{{item.zoneName}}</div>
                        <div class="time">
                            <span>
                                {{item.zoneTypeName}}
                            </span>
                        </div>
                    </div>
                </router-link>
            </section>
            
            <div class="clear"></div>
            <el-pagination
                v-if="total"
                class="page-box"
                @current-change="pageChange"
                layout="prev, pager, next"
                :page-size="pageSize"
                :total="total">
            </el-pagination>
        </template>
        <!-- 无内容 -->
        <section v-else class="no-img">
            当前城市暂无商圈，请更换城市！！！
        </section>
    </div>
</template>
<script>
import util from '../../../assets/common/util'
import { mapGetters } from 'vuex'

export default {
    data() {
        return {
            keyValue: '',
            // 获取
            sourceDatas: [],
            pageNumber: 1,
            pageSize: 15,
            total: 0,
            // 添加
            operateText: '添加',
            isAddItem: false,
            addItemForm: {
                enterpriseCode: '',
                zoneName:'',
                zoneType:'',
                zoneDesc:'',
                zoneCover:'',
                cityCname:'',
                zoneCode:'',
                cityCode:''
            },
            tradingAreaTypes: []
        }
    },
    mounted () {
        this.getItems()
    },
    computed: {
        ...mapGetters({
            userInfo: 'getUserInfo',
            selectCity: 'getSelectCity'
        })
    },
    watch: {
       selectCity () {
           this.getItems()
       } 
    },
    methods: {
        searchItem () {
            this.getItems()
        },
        // 增删改查
        getItems () {
            if (!this.selectCity.cityCode) {
                return false
            }

            var formData = {
                cityCode: this.selectCity.cityCode,
                pageSize: this.pageSize,
                pageNumber: this.pageNumber
            }

            if (this.keyValue) {
                formData.zoneName = this.keyValue
            }

            util.request({
                method: 'get',
                interface: 'cityZoneList',
                data: formData
            }).then(res => {
                if (res.result.success == '1') {
                    this.total = Number(res.result.total)
                    this.sourceDatas = res.result.result
                } else {
                    this.$message.error(res.result.message)
                }
            }) 
        },
        pageChange (size) {
            this.itemPageNumber = size
            this.getItems()
        }
    }
}
</script>
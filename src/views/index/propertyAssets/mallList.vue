<template>
    <div class="image-su-box">
        <!-- 目录 -->
        <div class="clear"></div>
        <!-- 内容 -->
        <template v-if="sourceDatas.length">
            <router-link v-for="(item, index) in sourceDatas"
                        class="check-box"
                        target="_blank"
                        :to="{
                            name: 'mall-detail',
                            query: {
                                enterpriseCode: userInfo.enterpriseCode,
                                mallCode: item.mallCode,
                                cityName: $route.query.cityName
                            }
                        }">
                
                <section class="sou-box">
                    <!-- 图片 -->
                    <div class="cover-box">
                        <img class="cover-img" :src="item.mallCover">
                    </div>
                    <div class="title-box">
                        <div class="title" v-text="item.mallName"></div>
                    </div>
                </section>
            </router-link>
        </template>
        <section v-if="!sourceDatas.length" class="no-img">
            暂无内容！！！
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
    </div>
</template>
<script>
import util from '../../../assets/common/util'
import { mapGetters } from 'vuex'

export default {
    data() {
        return {
            sourceDatas: [],
            isCheck: false,
            isAddItem: false,
            addItemForm: {
                zoneCode: '',
                mallAddr: '',
                mallGps: '',
                mallName: '',
                mallCover: ''
            },
            zoneDatas: [],
            zoneTotal: '',
            zonePageSize: 15,
            zonePageNumber: 1,
            checkedLabel: '',
            isselectVisible: false,
            // 选中的图片
            selectItemList: [],
            pageNumber: 1,
            pageSize: 15,
            total: 0
        }
    },
    mounted () {
        this.getItems()
        this.getZones()
    },
    computed: {
        ...mapGetters({
            userInfo: 'getUserInfo'
        })
    },
    methods: {
        getItems () {
            util.request({
                method: 'get',
                interface: 'mallInfoList',
                data: {
                    enterpriseCode: this.$route.query.enterpriseCode,
                    zoneCode: this.$route.query.zoneCode,
                    pageNumber: this.pageNumber,
                    pageSize: this.pageSize
                }
            }).then(res => {
                if (res.result.success == '1') {
                    this.sourceDatas = res.result.result
                } else {
                    this.$message.error(res.result.message)
                }
            })       
        },
        pageChange (size) {
            this.pageNumber = size
            this.getItems()
        }
    }
}
</script>
<template>
    <div class="image-su-box">
        <template v-if="sourceDatas.length">
            <router-link v-for="(item, index) in sourceDatas"
                        class="check-box"
                        target="_blank"
                        :to="{
                            name: 'reits-detail',
                            query: {
                                enterpriseCode: userInfo.enterpriseCode,
                                foreignReitsCode: item.foreignReitsCode
                            }
                        }">
                
                <section class="sou-box">
                    <!-- 图片 -->
                    <div class="cover-box">
                        <img class="cover-img" :src="item.foreignReitsCover">
                    </div>
                    <div class="title-box">
                        <div class="title" v-text="item.foreignReitsName"></div>
                        <span class="time">
                            {{item.securityTypeName}}
                        </span>
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
                exchangeCode: '',
                foreignReitsCode: '',
                assetManagementProductName: '',
                foreignReitsName: '',
                foreignReitsCover: '',
                securityType: ''
            },
            pageNumber: 1,
            pageSize: 15,
            total: 0
        }
    },
    mounted () {
        this.getItems()
    },
    computed: {
        ...mapGetters({
            userInfo: 'getUserInfo',
            selectExchange: 'getSelectExchange'
        })
    },
    watch: {
       selectExchange () {
           this.getItems()
       } 
    },
    methods: {
        getItems () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                exchangeCode: this.$route.query.exchangeCode,
                stockStatus: '1',
                pageNumber: this.pageNumber,
                pageSize: this.pageSize
            }

            if (this.selectExchange.exchangeCode) {
                formData.exchangeCode = this.selectExchange.exchangeCode
            }

            util.request({
                method: 'get',
                interface: 'foreignReitsInfoList',
                data: formData
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
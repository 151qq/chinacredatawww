<template>
    <div class="enterprise-list-box">

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
            <section class="sou-box" v-for="(item, index) in sourceDatas" :key="index">
                <router-link class="cover-box"
                             target="_blank"
                             :to="{
                                name: 'enterprise-info',
                                query: {
                                    enterpriseCode: item.enterpriseCode
                                }
                             }">
                    <img :src="item.enterpriseLogo">
                </router-link>
                <div class="title-box">
                    <div class="title" v-text="item.enterpriseNameReg"></div>
                </div>
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
            当前暂无内容，请添加！！！
        </section>
    </div>
</template>
<script>
import util from '../../../assets/common/util'

export default {
    props: ['productType'],
    data() {
        return {
            keyValue: '',
            // 获取
            sourceDatas: [],
            pageNumber: 1,
            pageSize: 24,
            total: 0
        }
    },
    mounted () {
        this.getItems()
    },
    methods: {
        searchItem () {
            this.getItems()
        },
        // 增删改查
        getItems () {
            var formData = {
                status: '0',
                pageNumber: this.pageNumber,
                pageSize: this.pageSize
            }

            if (this.keyValue) {
                formData.enterpriseCname = this.keyValue
            }

            util.request({
                method: 'get',
                interface: 'enterpriseList',
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
            this.pageNumber = size
            this.getItems()
        }
    }
}
</script>
<style lang="scss">
.enterprise-list-box {
    position: relative;
    width: 1000px;
    margin: 80px auto 30px;

    .input-box {
        display: block;
        width: 800px;
        height: 50px;
        margin: 0 auto 30px;

        .input-search {
          float: left;
          width: 600px;
          height: 50px;
          appearance: none;
          font-size: 14px;
          background-color: #fff;
          background-image: none;
          border-radius: 4px;
          border: 1px solid #bfcbd9;
          box-sizing: border-box;
          color: #1f2d3d;
          font-size: inherit;
          line-height: 1;
          outline: 0;
          padding: 3px 10px;
        }

        .search-btn {
          position: relative;
          float: left;
          margin-left: -10px;
          border-top-left-radius: 0;
          border-bottom-left-radius: 0;
          height: 50px;
          font-size: 16px;
          padding: 0 23px;
        }

        .add-new-btn {
            float: right;
            height: 50px;
        }
    }

    .page-box {
        margin-top: 10px;
    }

    .no-img {
        text-align: center;
        line-height: 160px;
        font-size: 20px;
        color: #999999;
    }

    .go-back {
        height: 30px;
        position: absolute;
        right: -30px;
        top: -38px;
        padding: 0 15px;
    }

    .sou-box {
        float: left;
        width: 105px;
        margin-right: 20px;
        margin-bottom: 20px;
        border: 1px solid #D3DCE6;
        border-radius: 3px;

        .cover-box {
            height: 105px;
            overflow: hidden;
            cursor: pointer;
            
            img {
                display: block;
                width: 100%;
                min-height: 105px;
            }
        }

        .title-box {
            display: flex;
            padding: 5px;
            border-top: 1px solid #D3DCE6;
            
            .title {
                flex: 1;
                font-size: 14px;
                line-height: 20px;
                border: none;
                color: #000000;
                overflow: hidden;
                text-overflow: ellipsis;
                white-space: nowrap;
            }

            .time {
                display: block;
                font-size: 14px;
                height: 20px;
                line-height: 20px;
                color: #999999;
            }

            .btn-box {
               float: right;
               font-size: 14px;
               color: #333333;
               margin-left: 5px;

               i, label {
                    cursor: pointer;
               }

               &:hover {
                    color: #20a0ff;
               }
            }
        }
    }
}
</style>
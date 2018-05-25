<template>
    <div class="auto-box">
        <router-link class="nav-btn"
                        v-if="searchData[mCode] && sName == 'enterpriseNameReg'"
                        target="_blank"
                        :to="{
                          name: 'enterprise-info',
                          query: {
                            enterpriseCode: searchData[mCode]
                          }
                        }">
            {{searchData[mName]}}
        </router-link>
        <span class="nav-btn" v-if="sName != 'enterpriseNameReg'">
            {{searchData[mName]}}
        </span>
    </div>
    
</template>
<script>
export default {
    props: ['searchData', 'mCode', 'mName', 'mType', 'sCode', 'sName', 'sType', 'fetchSuggestions'],
    data () {
        return {}
    },
    methods: {
        handleSelect (item) {
            if (this.mCode) {
                this.searchData[this.mCode] = item[this.sCode]
            }
            if (this.mName) {
                this.searchData[this.mName] = item[this.sName]
            }
            if (this.mType) {
                this.searchData[this.mType] = item[this.sType]
            }
        },
        fetchDatas (queryString, cb) {
            this.fetchSuggestions(queryString, cb)
        }
    }
}
</script>
<style lang="scss">
.auto-box {
    width: 100%;

    .nav-btn {
        display: block;
        width: 100%;
        border-radius: 4px;
        border: 1px solid #bfcbd9;
        box-sizing: border-box;
        color: #1f2d3d;
        font-size: 14px;
        height: 30px;
        line-height: 24px;
        padding: 3px 10px;

        &:hover {
            color: #20a0ff;
        }
    }
}

.input-box {
    .nav-btn {
        display: block;
        width: 100%;
        border-radius: 0px;
        border: none;
        box-sizing: border-box;
        color: #1f2d3d;
        font-size: 14px;
        height: 30px;
        line-height: 24px;
        padding: 0;
    }
}
</style>
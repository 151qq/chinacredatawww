<template>
  <div class="input-out" @click.stop="showModel">
    <div class="title-input-box">
      <el-input 
        placeholder="请输入内容"
        v-model="keyValue"
        class="title-input"
        @change="getMess"
        >
      </el-input>
    </div>
    <div class="mess-box" v-show="messDate.length !== 0 && isShow">
      <a v-for="item in messDate" class="nav-r" @click.stop="goMap(item)">{{ item.title }}</a>
    </div>
  </div>
</template>
<script>
import $ from 'Jquery'
import util from '../../assets/common/util'

export default {
  props: ['city'],
  data () {
    return {
      isShow: false,
      keyValue: '',
      messDate: [],
      count: 0
    }
  },
  created () {
    var _self = this
    $('body').click(function () {
      _self.messDate = []
      _self.isShow = false
    })
  },
  methods: {
    getMess () {
      if (this.keyValue === '') {
        return false
      }
      // getMess
      let _self = this
      this.isShow = true
      var options = {
        onSearchComplete (results) {
          // 更新调用记录
          _self.count++
          var arrs = [];      
          for (var i = 0; i < results.getCurrentNumPois(); i ++){
              arrs.push({
                title: results.getPoi(i).title,
                address: results.getPoi(i).address,
                point: results.getPoi(i).point
              })      
          }
          _self.messDate = arrs
        }
      }
      
      var local = new window.BMap.LocalSearch(this.city ? this.city : '北京', options)
      local.search(this.keyValue)
    },
    showModel () {
      this.isShow = true
    },
    hideMess (e) {
      this.isShow = false
    },
    goMap (item) {
      this.keyValue = item.title
      this.isShow = false
      this.$emit('mapChange', item)
    },
    resetKey (key) {
      if (key) {
        this.keyValue = key
        return
      }
      this.keyValue = ''
    }
  }
}
</script>
<style lang="scss">
  .input-out {
    position: relative;
    width: 100%;
    display: inline-block;

    .title-input-box {
      display: flex;

      .title-input {
        width: 100%;

        input {
            width: 100%;
            height: 36px;
        }
      }
    }

    .mess-box {
      position: absolute;
      left: 0;
      top: 36px;
      width: 100%;
      box-sizing: border-box;
      padding: 15px 0;
      border: 1px solid #C0CCDA;
      text-align: left;
      background: #ffffff;
      z-index: 10;
      
      .nav-r {
        display: block;
        padding: 0 15px;
        font-size: 14px;
        line-height: 30px;
        color: #333333;
        cursor: pointer;

        &:hover {
          background: #C0CCDA;
        }
      }
    }

    .el-input {
      display: inline-block;
    }

    .search-btn {
      position: relative;
      float: right;
      margin-left: -10px;
      border-top-left-radius: 0;
      border-bottom-left-radius: 0;
      height: 36px;
      font-size: 16px;
      padding: 0 23px;
    }
  }
</style>
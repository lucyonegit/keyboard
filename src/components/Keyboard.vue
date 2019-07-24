<template>
  <div class="keyboard" v-show="showKeyboard" v-clickoutside="closeModal">
    <p v-for="(keys ,index) in keyList" :key="index">
      <template v-for="(key,index) in keys" >
        <i
        :key="index"
          v-if="key === 'top'"
          @click.stop="clickKey"
          @touchend.stop="clickKey"
          class="iconfont icon-zhiding tab-top"
        ></i>
        <i
        :key="index"
          v-else-if="key === '123'"
          @click.stop="clickKey"
          @touchend.stop="clickKey"
          class="tab-num"
        >123</i>
        <i
        :key="index"
          v-else-if="key === 'del'"
          @click.stop="clickKey"
          @touchend.stop="clickKey"
          class="iconfont icon-delete key-delete"
        ></i>
        <i
        :key="index"
          v-else-if="key === '*+='"
          @click.stop="clickKey"
          @touchend.stop="clickKey"
          class="symbol"
        >*+=</i>
           <i
        :key="index"
          v-else-if="key === 'abc'"
          @click.stop="clickKey"
          @touchend.stop="clickKey"
          class="piny"
        >abc</i>
        <i
        :key="index"
          v-else-if="key === 'blank'"
          @click.stop="clickKey"
          @touchend.stop="clickKey"
          class="tabBlank"
        >空格</i>
        <i
        :key="index"
          v-else-if="key === 'enter'"
          @click.stop="clickKey"
          @touchend.stop="clickKey"
          class="tabEnter"
        >确定</i>
        <i :key="index" v-else @click.stop="clickKey" @touchend.stop="clickKey">{{key}}</i>
      </template>
    </p>
  </div>
</template>

<script>
import clickoutside from '../directives/clickoutside';

export default {
  directives: { clickoutside },
  data() {
    return {
      keyList: [],
      status: 0, //0 小写； 1 大写 ；2 数字 ；3 符号
      changeStatus:0,
      ungetFunction: ['point', 'symbol'],
       //小写
      lowercase: [
        ['q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p'],
        ['a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l'],
        ['top', 'z', 'x', 'c', 'v', 'b', 'n', 'm', 'del'],
        ['123', 'blank', 'enter']
      ],
      //特殊符号
      symbol: [
        ['[', ']', '{', '}', '#', '%', '^', '*', '+', '='],
        ['_', '\\', '|', '~', '<', '>', '$', '&', '@', '"'],
        ['123', '.', ',', '?', '!', "'", 'del'],
        ['abc', 'blank', 'enter']
      ],
       //数字
      numMode: [
        ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0'],
        ['-', '/', ':', ';', '(', ')', '$', '&', '@', '"'],
        ['*+=', '.', ',', '?', '!', "'", 'del'],
        ['abc', 'blank', 'enter']
      ],
       //大写
      uppercase: [
        ['Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P'],
        ['A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L'],
        ['top', 'Z', 'X', 'C', 'V', 'B', 'N', 'M', 'del'],
        ['123', 'blank', 'enter']
      ],
      equip: !!navigator.userAgent.toLocaleLowerCase().match(/ipad|mobile/i) //是否是移动设备
    };
  },
  props: {
    option: {
      type: Object
    }
  },
  computed: {
    showKeyboard() {
      return this.option.show;
    }
  },

  mounted() {
    this.keyList = this.lowercase;
  },

  methods: {
    tabHandle(event,{ value = '' }) {
      if (value.indexOf('tab-num') > -1) {
        this.changeStatus = this.status //保存之前的大小写状态
        this.status = 2;
        this.keyList = this.numMode;
        //数字键盘数据
      }
      else if(value.indexOf('symbol') > -1){
        this.status = 3
        this.keyList = this.symbol
      } 
      else if(value.indexOf('piny') > -1){
        switch(this.changeStatus){
          case 0:
            this.status = 0;
            this.keyList = this.lowercase;
            break
          case 1:
            this.status = 1;
            this.keyList = this.uppercase;
            break
          default:
            this.status = 0;
            this.keyList = this.lowercase;
        }
       
      }else if (value.indexOf('key-delete') > -1) {
        this.emitValue('delete');
      } else if (value.indexOf('tabBlank') > -1) {
        this.emitValue(' ');
      } else if (value.indexOf('tabEnter') > -1) {
        this.closeModal(event)
        //this.emitValue('\n');
      } else if (value.indexOf('tab-top') > -1) {
        if (this.status === 0) {
          this.status = 1;
          this.keyList = this.uppercase;
        } else {
          this.status = 0;
          this.keyList = this.lowercase;
        }
      } else {
      }
    },
    clickKey(event) {
      if (event.type === 'click' && this.equip) return;
      let value = event.srcElement.innerText;
      value && value !== '空格' && value !== '123' && value !== '确定' && value!=="*+=" && value !== 'abc' 
        ? this.emitValue(value)
        : this.tabHandle(event,event.srcElement.classList);
    },

    emitValue(key) {
      this.$emit('keyVal', key);
    },

    closeModal(e) {
      if (e.target !== this.option.sourceDom) {
        // this.showKeyboard = false
        this.$emit('close', false);
      }
    }
  }
};
</script>
<style scoped lang="less">
.keyboard {
  width: 100%;
  margin: 0 auto;
  font-size: 0.36rem;
  border-radius: 2px;
  padding-top: 0.5em;
  background-color: #e5e6e8;
  user-select: none;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 999;
  pointer-events: auto;
  p {
    width: 95%;
    margin: 0 auto;
    height: 0.9rem;
    margin-bottom: 0.5em;
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: center;
    i {
      display: block;
      margin: 0 1%;
      height: 0.9rem;
      line-height:0.9rem;
      font-style: normal;
      font-size: 0.48rem;
      border-radius: 3px;
      width: 44px;
      background-color: #fff;
      text-align: center;
      flex-grow: 1;
      flex-shrink: 1;
      flex-basis: 0;
      -webkit-box-flex: 1;
      &:active {
        background-color: darken(#ccc, 10%);
      }
    }
    .tab-top {
      width: 1rem;
      margin: 0 1%;
      background: #cccdd0;
      color: #fff;
      font-size: 24px;
    }
    .key-delete {
      width: 1rem;
      margin: 0 1%;
      color: #827f7f;
      background: #d7d7d8;
    }
    .tabNum {
      font-size: 0.44rem;
      flex:1;
      background: #dedede;
      color: #5a5959;
      line-height: 0.9rem;
    }
    .tabBlank {
      flex:2;
      font-size: 0.44rem;
      padding: 0 0.3rem;
      color: #5a5959;
      line-height: 0.9rem;
    }
    .tabEnter {
       flex:1;
      font-size: 0.44rem;
      line-height: 0.9rem;
      background: rgb(20,111,223);
      color: #fff
    }
    &:nth-child(2) {
      width: 88%;
    }
  }
}
</style>

<template>
  <div class="keyboard" v-show="showKeyboard" v-clickoutside="closeModal">
    <p v-for="(keys ,index) in keyList" :key="index">
      <template v-for="(key,index) in keys" >
        <i
        :key="index"
          v-if="key === 'del'"
          @click.stop="clickKey"
          @touchend.stop="clickKey"
          class="iconfont icon-delete key-delete"
        ></i>
         <i
        :key="index"
          v-else-if="key === '0'"
          @click.stop="clickKey"
          @touchend.stop="clickKey"
          class="zero"
        >0</i>
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
      status: 2, //0 小写； 1 大写 ；2 数字 ；3 符号
       //数字
      numMode: [
        ['1', '2', '3'],
        ['4', '5', '6'],
        ['7', '8', '9'],
        ['enter','0','del']
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
    this.keyList = this.numMode;
  },

  methods: {
    tabHandle(event,{ value = '' }) {
      if (value.indexOf('key-delete') > -1) {
        this.emitValue('delete');
      } else if (value.indexOf('tabEnter') > -1) {
        this.closeModal(event)
      }else {
      }
    },
    clickKey(event) {
      if (event.type === 'click' && this.equip) return;
      let value = event.srcElement.innerText;
      value && value !== '确定'
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
      line-height: 0.9rem;
      font-style: normal;
      font-size: 0.48rem;
      border-radius: 3px;
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
    .key-delete {
      font-size: 0.44rem;
      line-height: 0.9rem;
      flex:1;
      color: #827f7f;
      background: #d7d7d8;
    }
    .zero{
        flex:1;
    }
    .tabEnter {
      font-size: 0.44rem;
      line-height: 0.9rem;
       flex:1;
      background: rgb(20,111,223);
      color: #fff
    }
  }
}
</style>

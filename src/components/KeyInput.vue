<template>
  <div>
    <!-- <input type="text" ref="Numkeyboard" v-model="inputNum" @focus="onNumFocus" />
    <input type="password" ref="keyboard" v-model="inputValue" @focus="onFocus" /> -->
    <div class="logo">
      <img :src="LOGO" alt="">
    </div>
    <div class="dempinput username" ref="Numkeyboard"  @click="()=>{onNumFocus('Numkeyboard')}">{{inputNum}}</div>
    <div class="dempinput password" ref="keyboard"  @click="()=>{onFocus('keyboard')}">{{showPass}}</div>
    <div :class="[submitTouch?'submitBtn touchsubmitBtnBg':'submitBtn']" @touchstart="submitTouch = true" @touchend="()=>{submitTouch = false;submit()}" >登录</div>
    <div class="other">
      <a href="/">忘了密码 ？</a>
      <a href="/">注册新账号</a>
    </div>
    <Keyboard :option="option" @keyVal="getInputValue" @close="option.show = false"></Keyboard>
    <NumkeyBoard :option="Numoption" @keyVal="getNumInputValue" @close="Numoption.show = false"></NumkeyBoard>
  </div>
</template>
<script>
import Keyboard from '../components/Keyboard';
import NumkeyBoard from '../components/NumKeyboard'
import LOGO from '../assets/que.png'
export default {
  components: {
    Keyboard,NumkeyBoard
  },
  computed:{
    LOGO(){
      return LOGO
    },
     showPass:function(){
       if(this.inputValue === '请输入你的qq密码'){
         return '请输入你的qq密码'
       }
      return new Array(this.inputValue.length).fill('*').join("")
    }
  },
  data() {
    return {
      option: {
        show: false,
        sourceDom: ''
      },
      Numoption: {
        show: false,
        sourceDom: ''
      },
      submitTouch:false,
      inputValue: '请输入你的qq密码',
      inputNum:'手机号码/手机',
    };
  },
  props: {},
  created() {},
  methods: {
    getInputValue(val) {
      if (val === 'delete') {
        if(this.inputValue === "请输入你的qq密码"){
           return 
         }
        this.inputValue = this.inputValue.slice(0, this.inputValue.length - 1);
        if(this.inputValue === ''){
          this.inputValue = '请输入你的qq密码'
        }
      } else {
         if(this.inputValue.includes('请输入你的qq密码')){
          this.inputValue = ''
        }
        this.inputValue += val;
      }
    },
    getNumInputValue(val){
       if (val === 'delete') {
         if(this.inputNum === "手机号码/手机"){
           return 
         }
        this.inputNum = this.inputNum.slice(0, this.inputNum.length - 1);
        if(this.inputNum === ''){
          this.inputNum = '手机号码/手机'
        }
      } else {
        if(this.inputNum.includes('手机号码/手机')){
          this.inputNum = ''
        }
        this.inputNum += val;
      }
    },
    onFocus(ref) {
       this.option = {
        show: true,
        sourceDom:  this.$refs['keyboard']
      }
    },
    onNumFocus:function(ref){
      // this.$refs[ref].innerText = ''
      this.Numoption = {
        show: true,
        sourceDom:  this.$refs['Numkeyboard']
      }
    },
    submit(){
      console.log({inputValue:this.inputValue.includes('请输入你的qq密码')?'':this.inputValue,inputNum:this.inputNum.includes('手机号码/手机')?'':this.inputNum})
    }
  }
};
</script>
<style lang="less" scoped>
.logo {
  width: 65%;
  max-width: 412px;
  height: 2rem;
  margin:1rem 0 0.4rem 0;
}
.logo img{
  height: auto;
  width:100%;
}
.dempinput{
  width: 80%;
  box-sizing: border-box;
  text-align:left;
  padding: 0px 0.4rem;
  line-height: 1rem;
  background: #fff;
  color:#888;
  font-size: 0.32rem;
}
.username{
  border-radius: 4px 4px 0 0;
  border-bottom: 1px solid #ddd
}
.password{
   border-radius:0 0 4px 4px
}
.submitBtn{
  width: 80%;
  height: 1rem;
  line-height: 1rem;
  background: rgb(20,111,223);
  border-radius: 4px;
  margin-top: 0.2rem;
  color: #fff;
  font-size:0.36rem;
}
.touchsubmitBtnBg{
  background: rgb(87, 163, 255);
}
.other{
  width: 80%;
  margin-top: 0.4rem;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  a{
    text-decoration: none;
    color:#246183;
    font-size: 0.3rem;
  }
}
</style>

# project-vue2.0
vue2.0简单项目心得

  当父组件给子组件（弹窗）传值是json数组（json包数组），用来渲染弹窗里面的数据时，第一次数据总是渲染不上去。用下面的方式传就可以，我目前还不清楚原因
  api.customer_allLableList(param2)
    .then((respond) => {
      let tempObj = {},objShow={},arr= {};
      if (respond.status === 'success') {
        for (let i = 0; i < respond.data.length; i++) {
          let temp = '';
          if (respond.data[i].key.indexOf('意向等级') > 0) {
            temp = 'itention'
          } else if (respond.data[i].key.indexOf('片区') > 0) {
            temp = 'regionData'
          //此处...
          } else if (respond.data[i].key.indexOf('月消费') > 0) {
            temp = 'cost'
          } else if(respond.data[i].key.indexOf('户型') > 0) {
            temp = 'houseType'
          }
          tempObj[temp] = respond.data[i].value;
          objShow[temp] = true;
        }
      }
      arr.lableList=tempObj;
      arr.objShowArr=objShow;
      return arr;
    })
    .then((data) => {
      this.customerDetailList = data.lableList;         //这样传的值才能在组件里面正常传过去
      this.tempObjShow = data.objShowArr;
    })
  核心是在请求回来后得到的数组里，用return 出来，再在.then()里面接受再赋值处理


  父组件给子组件传值来控制子组件某个开发的开与关时，两个页面明明用的是同一套数据，但是有一个是好的，一个不是正常的。
  <el-switch v-model="customerDetailInfo.closeFlagValue"
             @change="closeFlagSubmit"
             on-text=""
             off-text="">
  </el-switch>
  <span v-if="isflag">已成交</span>
  <span v-else>未成交</span>
  js里面
  updated() {
    this.isflag = this.customerDetailInfo.closeFlagValue;
  },
  methods: {
    closeFlagSubmit(val) {
      this.$emit('closeFlagSubmit', this.customerDetailInfo.closeFlagValue);
      this.isflag = val;
    },
  }
  另外定义一个变量来控制就正常了。不清楚原因。。。。。
 
 此次项目用到了element.ui框架，有几个地方我需要注意一下：
 1、当一个页面tab切换时，每次切换都有表格。切换的表格都会有变化时会导致表格布局错乱。想用一个表格来用v-if来改变表格字段显示与否。可行的前提条件是tab切换的表格只有一个选项是动态的。否则就用不了这种方法。因为我涉及的是好几个字段的判断，所以我就无奈的采用了element提供的el-tabs。三个页面就要写三张表格。
 2、element.ui的表格若想自适应宽度的话，至少要有一项是不能定宽。不设置宽的那些选项会将剩下的宽度均分。
 
 要在打包时的静态资源前面加一串路径，应该在config里面的index.js的module.exports》build》assetsPublicPath中设置


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

#### 系统接线图实时数据部分(SystemDiagram.js)
 监听 `point-value-event` 事件拿到实时更新的数值后要根据标签的类名选中相应的元素填充相应的显示数值或者进行样式的更改,这里应注意显示的数值希望在变化的时候有闪烁的效果,另外一件事是把接收到的实时值对初始值进行替换,组成新数组作为传递给子组件的数据.可得知这个事件要做的事情还是蛮多的,为了方便维护,我们就要对这些要做的事情进行拆分,分成各个函数分别执行独立的事情.
 ###### 当实时值有更新时,出现闪烁的效果更新在页面上
 闪烁效果的实现就是当实时值更新时,先把展示的内容至空再间隔几秒后渲染实时值就能出现闪烁的效果
 - 设置5秒钟一次随机更新页面的显示值为空的定时器
 `setInterval(this.resetChartData, 5000)`
 这个定时器5秒钟一次随机的把一些点的值设为空
 ```
   resetChartData () {
    let len = this.refreshPoint.length
    let randomNum = Math.floor(Math.random() * (len > 20 ? 20 : len))
    for (let i = 0; i < len; i++) {
      i % randomNum === 0 && $('.point-text-' + this.refreshPoint[i].key).html('')
    }
    this.updateChartData(this.refreshPoint)
  }
  ```
  这个随机数不能大于20是为了防止每次更新的点数量过少
  在随机置空之后调用this.updateChartData方法更新正确的实时值

  - 更新实时值
  更新实时值时设置了0.3秒的一次性定时器,也是为了闪烁效果.页面上更新实时值或者更新显示效果的操作都放在这个函数中实现.
  ```
   updateChartData (data) {
    setTimeout(() => {
      data.forEach(item => {
        $('.point-text-' + item.key).html(getValueLabel(item.value, 2))
        !!this.subscibeRect.length && this.subscibeRect.forEach(point => {
          if (point === item.key) {
            item.value === 0 ? $('.point-c-switch-' + point).addClass('styleB-value') : $('.point-c-switch-' + point).addClass('styleC-value')
          }
        })
      })
    }, 300)
  }
  ```
  - 在实时值更新时把更新的实时值至为空
  当实时值更新时要把页面中对应的实时值至为空,把实时值更新到存放他的数组中去
  ```
    clearChartData (msg) {
    msg.forEach(item => {
      let pointIndex = this.refreshPoint.find(data => data.key === item.key)
      if (pointIndex > 0) {
        this.refreshPoint[pointIndex].value = item.value
      } else {
        this.refreshPoint.push(item)
      }
      $('.point-text-' + item.key).html('')
    })
  }
  ```
  - 整合实时值传给子组件
  ```
    updateModalData (msg) {
    let popPoint = [...this.props.circuitPoint]
    msg.forEach(item => {
      let index = popPoint.findIndex(data => data.key === item.key)
      if (index > 0) {
        popPoint[index].value = item.value
        popPoint[index].ts = item.ts
      } else {
        popPoint.push(item)
      }
    })
    let temp = (popPoint[0] || {}).circuit
    this.setState({
      circuitDes: temp ? {name: temp.name, id: temp.id} : {},
      telemeter: sortPoints(popPoint.map(item => {
        return {name: item.name, value: item.value, ts: item.ts, type: item.type}
      }))
    })
  }
  ```
  把拿到的实时值进行拼接组合成为传给子组件的数据
  - 最终调用
  把每一步操作都拆开来写,在监听事件里面调用,这样就清晰多了
  ``
      socket.on('point-value-event', (msg) => {
      this.clearChartData(msg)
      this.updateChartData(msg)
      this.updateModalData(msg)
    })
    ```
#### 导航组件(DefaultLayout.js)
- 组件结构
导航分为三部分左边的logo区,中间的导航区,右边的用户区.
- 导航部分结构
中间的导航区又为一级导航二级导航.
每个一级导航使用<NavDropdown>,并添加onMouseEnter事件,当前高亮并展示出该导航下的二级菜单;onDoubleClick事件快速进入该导航下第一个二级导航.
每组二级导航单独抽出组件,其中每个二级导航点击跳转并相应一级菜单高亮
- 导航权限

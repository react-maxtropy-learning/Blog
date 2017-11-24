#### 由props赋值的对象给了一个属性值,但是读不出来
  ```
  let data =  this.props.arr// 一个数组
  data[0].value = 6 //现在给第一项一个value值
  data[0].value//取不出来值
  ```
  两个的指针指向同一个地址,在赋值的时候需要注意
  改写为`let data = [...this.props.arr]`

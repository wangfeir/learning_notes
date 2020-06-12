### 接口
#### 1、实现一个简单的接口

```javascript
// 接口的命名首字母大写
interface LabelValue{
	label:string
}

// 在函数中使用接口定义传入参数的类型，只要你传入的参数重包含了接口里面已经定义的所有值就不会报错，而且不管传参的顺序是不是按照接口的顺序都是可以的
function printLabel(lebalObj:LabelValue){
  console.log(lebalObj.label)
}

const myObj = {
  size:10,
  label:'my label value 10'
}
printLabel(myObj);
```

#### 2、可选参数接口

```javascript
// 可选接口
// 接口内定义的参数是否存在都是可以的
interface CircularArea {
  color?:string
  width?:number
}

function circular(myData:CircularArea):string{
  let newObj = {color:'yellow',width:200}
  if(myData.color){
    newObj.color = myData.color
  }
  if(myData.width){
    newObj.width = myData.width
  }
  return `这个圆形的颜色是${newObj.color},面积是：${newObj.width *newObj.width }`
}
// 接口的参数是可选的 
console.log(circular({color:'blue'}))

// 加入传入的参数里面存在接口中没有定义的还是会报错
circular({color:'blue',area:2000}) // 会报错，因为没有定义过area这个字段
//解决这种报错的方法如下

interface CircularArea {
  color?:string
  width?:number
  [performance:string]:any  // 前面表示这个接口可以随意添加任何字符串格式的key:any表示值也可以是任何格式的 
}

```

#### 3、不定长度和参数的接口实现
```javascript
interface CircularArea {
  color?:string
  width?:number
  [performance:string]:any  // 前面表示这个接口可以随意添加任何字符串格式的key:any表示值也可以是任何格式的 
}

function circular(myData:CircularArea):string{
  let newObj = {color:'yellow',width:200}
  if(myData.color){
    newObj.color = myData.color
  }
  if(myData.width){
    newObj.width = myData.width
  }
  return `这个圆形的颜色是${newObj.color},面积是：${newObj.width *newObj.width }`
}

// 因为定义接口时可以允许添加添加任何内容和长度的参数所以这里不会报错
console.log(circular({color:'blue',colors:'yellow',height:200}))

```
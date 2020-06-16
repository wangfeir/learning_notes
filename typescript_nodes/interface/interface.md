### 接口
#### 1、实现一个简单的接口

```typescript
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

```typescript
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
```typescript
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

#### 4、接口中定义只读参数 
```typescript
interface OnlyArrey{
  readonly x:string //定义只读类型就是在参数的前面添加readonly
  readonly y:string
}
let myObj:OnlyArrey = {
  x:'1111',
  y:'2222',
}
myObj.x = 2222 // 接口中把这个值定义为只读类型是不可更改的，这里会报错
console.log(myObj)
//定义只读数组
let onlyArray:ReadonlyArray<number> = [1,2,3,34,4,5,7]
onlyArray[2] = 2 // 这是会报错的

```

#### 5、函数类型接口 
```typescript
// 函数类型接口
interface SearchFunc{
  (source:string,subString:string):boolean // 这里定义了一个函数类型的接口 接收两个参数 返回值是boolean类型
}
let mySearch: SearchFunc
// 在使用的时候 函数的传入值可以使用简写 但是类型必须一致
mySearch = function(sur:string,subStr:string):boolean{
  let search = sur.search(subStr)
  return search>-1
}

mySearch('hello','he')

```

#### 6、类类型接口
```typescript
// 类 类型接口
interface ColckInterface {
  // 定义一个参数
  currentTime:Date
  // 定义一个类中必有的方法
  setTime(d:Date)
}

// 使用接口创建一个类 
class Clock implements ColckInterface {
  currentTime:Date
  constructor(h:number){

  }
  // 创建接口中定义的方法 在方法里面修改currentTime的值
  setTime(d:Date){
    this.currentTime = d
  }
} 

```

#### 7、interface里的new
  * 如果签名有前缀new表示这个函数必须要被构造器调用
  * 简单说就是class里面那个constructor
  * 一般人不会在interface里面用new，因为class本来就能代表类型，并且还能作为值使用，为什么不用class还非要写个同名的interface
```typescript
  class fakeNum{
    constructor(_val:string){}
    static aa='xx'
    static bb =11
  }
  interface fakeNum2{
    new(val:string):fakeNum
    aa:string
    bb:number
  }
  let kk:fakeNum2 = fakeNum

```
* 这个kk就是fakeNum2的类型，也等于fakeNum的类型。所以没事谁又写个interface，只有写d.ts给别人用才会这么写。

#### 7、类类型接口-构造器的使用
```typescript

//  创建一个类类型接口
interface ClockInterface {
  tick()
}

// 创建一个类类型接口
interface ClockConstructor{
  // 创建一个构造器 类似于constructor 
  new(hour:number,minute:number):ClockInterface
}

// 创建一个方法 
// 这个方法接收的第一个参数：要求符合ClockConstructor接口，该接口中的构造函数要求符合ClockInterface接口
// 然后又定义了 这个方法的返回值必须符合 ClockInterface 接口 （:ClockInterface）
function createClock(ctor:ClockConstructor,hour:number,minute:number):ClockInterface{
  // 这里的return ctor 的方法必须含有两个参数，因为在 ClockConstructor 这个接口中定义了要接收两个参数
  return new ctor(hour,minute)
}

class DigitalClock implements ClockInterface{
  constructor(h:number,m:number){

  }
  tick(){
    console.log('beep beep')
  }
}

class AnalogClock implements ClockInterface{
  constructor(h:number,m:number){

  }
  tick(){
    console.log('tick tck')
  }
}
// 使用createClock方法 传入三个值：1、符合ClockConstructor接口的类 ，2、两个number类型的参数
let digital = createClock(DigitalClock,15,20)
let analogtal = createClock(AnalogClock,15,20)
digital.tick()

```
#### 9、接口的继承方法 extends
```typescript

// 接口的继承
interface Shape{
  color:string
}

interface PanStroke{
  penWidth:number
}

// 使用extends继承接口
// 让接口squre继承Shape和PanStroke两个接口
interface Squre extends Shape,PanStroke{
  sideLength:number
}

// 新建squre对象使用Squre接口
let squre = {} as Squre;

squre.color = 'red'
squre.sideLength = 10
squre.penWidth = 3.0

```
#### 10、接口继承类
```typescript
// 接口继承类

// 创建一个类并添加一个私有属性
class Control{
  private state:any

}

// 创建一个接口 继承 Control类的所有属性
interface SelecttableControl extends Control{
  // 定义一个select方法
  select()
}

// 创建一个类 extends(继承)自 Control 并implements(重写) SelecttableControl
class Button extends Control implements SelecttableControl{
  // 因为继承自 
  select(){
    console.log('select11')
  }
}
let button = new Button;
button.select()

// 创建一个类 extends 自Control 
class TextBox extends Control {
  select(){
    console.log('select222')
  }
}
let textBox = new TextBox;
textBox.select()

// 这个class是不被允许的 因为他无法通过implements 或者 extends 获取SelecttableControl父类Control中的私有属性
class ImageC implements SelecttableControl{
 
}


```
#### 泛型
- TS中泛型的实现使我们能够创建可重用的组件，一个组件可以支持多种类型的数据，为代码添加额外的抽象层和可重用性。泛型可以应用于TS中的函数、接口和类
```typescript
// 创建一个简单的function 接收number类型 返回 number类型
function identity(arg: number): number {
  return arg
}
// 如果不确定传入的类型时可以使用any类型
// 但是这种写法无法控制输入类型和输出类型的一致性验证
function identity1(arg: any): any {
  return arg
}
// 泛型变量
// 如果不能确定输入的类型和输出的类型 但是又想对一致性进行约束就要使用泛型变量
function identity2<T>(arg: T): T {
  return arg
}
// 使用方式
// 1、直接泛型变量：string 
let output = identity2<string>('myString');
// 2、不传入泛型变量让编辑器根据传入的参数的类型自行判断，但是在复杂的类型情况中有可能会出错
let output2 = identity2('myString')

// 除了直接使用的泛型变量还可以定义数组类型的泛型变量
// 这样的结果就是根据传入的泛型变量来定义一个符合变量的数组
function loggingIdentity<T>(arg: T[]): T[] {
  console.log(arg.length)
  return arg
}
// 接收和返回都是字符串类型的数组
let lg = loggingIdentity<string>(['one', 'two'])

```
#### 泛型类
```typescript
  // 创建一个泛型类
class GenericNumber<T>{
  // 创建一个静态变量 类型为泛型类的类型 
  zeroValue: T
  // 创建一个方法
  add: (x: T, y: T) => T
}

// 创建一个变量，类型为数字类型
let myGenericNumber = new GenericNumber<number>()
myGenericNumber.zeroValue = 0
myGenericNumber.add = function(x,y){
  return x+y
}
console.log(myGenericNumber.add(10,20)) // 30

// 创建一个变量，类型为string类型
let stringGenericNumber = new GenericNumber<string>()
stringGenericNumber.zeroValue = ''
stringGenericNumber.add= (x,y)=>{
  return x+y
}
console.log(stringGenericNumber.add(stringGenericNumber.zeroValue ,'text'))

```

#### 泛型约束

- 如果我们想要限制传入值必须带有某一个属性比如.numLengs属性。 只要传入的类型有这个属性，我们就允许，就是说至少包含这一属性。 


创建一个包含 .numLengs属性的类或者接口，使用这个类或者接口和extends关键字来实现约束：

```typescript
// 创建一个带有 numLengs 的类
class Animal{
  numLengs:number
}

class LionKeeper{
  nametag:string
}
// 创建一个类 继承 Animal
class Lion extends Animal{
  keeper:LionKeeper
}
// 创建一个新的类
class Bee {
  name:string
}
class Dog {
  name:string
  numLengs:number
}
// <T extends Animal>  传入的参数必须有 Animal 已有的属性
// (c:new()=>T) 传入的值 意即c的类型是对象类型且这个对象包含返回类型是T的构造函数。
// 注意，':'后面是TypeInformation，这里的'=>'不是arrowfunction(箭头函数)，只是用来标明函数返回类型
// :T 返回值必须和传入值类型一致
function createInstance<T extends Animal>(c:new()=>T):T{
  return new c()
}
createInstance(Lion).keeper
// createInstance(Bee)  // error 因为Bee不是继承自Animal 并且本身也没有numLengs的属性
createInstance(Dog) // 这样不会报错 因为Dog虽然不是继承自Animal但是本身有numLengs的属性
```
#### 1、命名函数与匿名函数
```typescript
// 命名函数
function add(x: number, y: number): number {
  return x + y
}
// 匿名函数

let myAdd : (x:number, y:number) => number = function(x:number, y:number) :number{
  return x + y
}

// 函数的类型推断
// 不定义一名对象的函数类型与传参类型 ts 也可以通过类型推断自己判断格式
// 这段代码等同与上面那种写法
let myAdd = function (x: number, y: number): number {
  return x + y
}
```

- 定义一个正常的函数
```typescript
function buildName(firstName: string, lastName: string): string {
  return firstName + ' ' + lastName
}

let result1 = buildName('Bad'); // error 没有lastName参数
let result2 = buildName('Bad', 'Adams', 'Sr.'); // error 应有两个参数传入了三个
let result3 = buildName('Bad', 'Adams'); // Bad Adams

```

- 可选参数
```typescript
function buildNameFun(firstName: string, lastName?: string): string {
  if (lastName) {
    return firstName + ' ' + lastName
  } else {
    return firstName
  }
}
let build1 = buildNameFun('Bad'); // Bad
let build2 = buildNameFun('Bad','Adams'); // Bad Adams
let build3 = buildNameFun('Bad','Adams','Sr.'); // err 应有 1-2 个参数，但获得 3 个

```

- 默认值
```typescript
// 默认值
function buildNameFunNew (firstName='Will',lastName:string):string{
  return firstName + ' ' + lastName
}

let buildNameNew1 = buildNameFunNew('Bad'); // err 应有 2 个参数，但获得 1 个
let buildNameNew2 = buildNameFunNew('Bad','Adams'); // Bad Adams
// 如果默认是第一个参数，如果想要使用那么第一个参数就要传入undefined，否则ts会认为你传入的是值是第一个参数
let buildNameNew3 = buildNameFunNew(undefined,'Adams'); // Will Adams
```

- 剩余参数

```typescript
// 剩余参数
// resetOfName会把其他的剩余参数放进一个数组中
function buildName(firstName: string, ...resetOfName: string[]): string {
  return firstName + ' ' + resetOfName
}

let result1 = buildName('Bad'); // Bad
let result2 = buildName('Bad', 'Adams', 'Sr.'); // Bad Adams,Sr.
let result3 = buildName('Bad', 'Adams'); // Bad Adams
```
#### 1、实现一个简单的类
```typescript
// 类
class Animal{
  name:string
  constructor(name:string){
    // constructor方法接收的参数就是调用该类时传入的参数
    // this.name 指向的是class自己创建的name 
    
    this.name = name
  }
  move( distance:number = 0){
    console.log(`${this.name} move ${distance}m`)
  }

}
// 创建一个类 继承 Animal
class Snake extends Animal{
  constructor(name:string){
    // 通过super这个关键字调用父类的构造函数(这是必须要做的) 将自己的name传递给父类constructor方法中需要的name
    super(name)
  }
  move(distance:number = 5){
    console.log('slithering')
    // 通过super.move() 来调用父类中的move方法并给其传入参数
    super.move(distance)
  }
}
class Horse extends Animal{
  constructor(name:string){
    super(name);
  }
  move(distance:number = 45){
    console.log('running')
    super.move(distance)
  }
}


let sam = new Snake('Sammy')
let tom = new Horse('Tom')
sam.move() // slithering  Sammy move 5m
tom.move(35) // running  Tom move 35m

```

#### 2、类的私有变量 private
```typescript
// 类的私有变量
class Animal{
  private name:string
  constructor(name:string){
    this.name = name
  }
  move(distands:number = 0){
    this.name = 'tom'  // 如果想要修改私有变量 只能通过class自身的方法里修改，子类也可以通过调用父类的方法进行修改
    console.log(`${this.name} move ${distands} m`)
  }
}

class Rhino extends Animal{
  constructor(){
    // 调用Animal的构造方法constructor，在父类的constructor中修改父类的私有变量，通过这种方法可以给父类的私有变量传参数
    super('Rhino'); 
  }
}

let rhino = new Rhino()
rhino.name = "dddd" // 会报错 因为Animal 的name 是私有的 不可以在外部被调用和修改
rhino.move(20)  // Rhino move 20 m
let dog = new Animal('Dog')
dog.name = 'Tom' // 会报错 因为Animal 的name 是私有的 不可以在外部被调用和修改
console.log(dog.name)  // 会报错 因为Animal 的name 是私有的 不可以在外部被调用和修改

```
#### 3、受保护的变量 protected
```typescript
// 类的私有变量
class Person {
  // 将name定义为 protected 受保护的 这样在函数外就不能访问这个值
  protected name: string
   // 如果对constructor也使用了protected那么 new Person() 也是会报错的
  constructor(name: string) { 
    this.name = name
  }
}

class Employee extends Person {
  protected department: string
  constructor(name: string, department: string) {
    super(name);
    this.department = department;
  }
  getElevatorPitch(){
    return `Hello, my name is ${this.name} and I work in ${this.department}`
  }
}

let haoward = new Employee('Howard',"sales");
console.log(haoward.getElevatorPitch()) // Hello, my name is Howard and I work in sales
console.log(haoward.name)  // 这里会报错，因为name 是 继承至 Person 而Person中的name是受保护的
let person = new Person('Tom') // 这是被允许的，但是如果constructor也使用了protected 那么这里也是会报错的
console.log(person.name) // 这样也是不被允许的，无论是继承的class还是直接new的class都不允许在外部访问受保护的值

```

#### 6、抽象类
- 用abstract关键字定义抽象类和抽象方法，抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。
- abstract抽象方法只能放在抽象类里面 

```typescript
// 使用abstract 定义一个抽象类
abstract class Department {
  name: string
  constructor(name: string) {
    this.name = name
  }
  printName(): void {
    console.log(`Department name ${this.name}`)
  }
  // 定义一个抽象方法 方法在必须在派生类中实现,否则会报错
  abstract printMeeting(): void
}

class AccountingDepartment extends Department {
  constructor() {
    super('Accounting ad Auditing')
  }
  printMeeting(): void {
    console.log('The Accounting Deepartment meets each Mondy at 10am')
  }
  genteraterReports(): void {
    console.log('Generating accounting reports....')
  }
}


let department: Department
department = new AccountingDepartment()
department.printName();
department.printMeeting();
department.genteraterReports();  // error  因为department的格式定义为Department，而抽象类中没有定义这个方法；
```
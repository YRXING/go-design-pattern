## 前言

高质量代码的要求：容易维护、容易扩展、容易复用。

重复的代码多到一定程度，维护的时候，可能就是一场灾难，在面向对象编程中，通过封装、继承、多态把程序的耦合度降低，用设计模式使得程序更加的灵活，容易修改，并且容易复用。

面向对象的编程，并不是类越多越好，类的划分是为了封装，但分类的基础是抽象，具有相同属性和功能的对象的抽象集合才是类。

### UML类图

设计模式必须搞懂的UML类图：

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gtn28gtm5kj60s10fjt9f02.jpg" alt="未命名文件 (1)" style="zoom:88%;" />



一个类中引用另一个类，是关联关系（可统一用箭头+实线表示），包含：

- 聚合（aggregation）：一种弱拥有关系，体现的是A对象可以包含B对象，但B对象不是A对象的一部分（比如雁群和大雁）
- 合成（composition）：也称为组合，是一种强拥有关系，体现的是严格的部分和整体，生命周期一样。（比如鸟和翅膀）

类中的方法参数如果包含其他类，称为依赖关系

### Golang如何实现OOP

因为Golang不属于面向对象语言，天生的面向对象的关键字和特性Go几乎都没有，但是可以优雅的使用Go语言来实现面向对象编程。Go语言是通过接口和匿名组合（也称内嵌结构体）来模仿继承和多态的行为，但严格意义上讲，匿名组合其实是关联关系，因为使用匿名组合的方法时候，可以省略结构体名，因此表现的好像是继承过来的一样，甚至还可以定义同名函数，对外就表现为了重写。

```go
type stu struct {
	name string
}

func (s stu) getName() string{
	return s.name
}

type middle struct {
	stu
}

func (m middle) getName() string{
	return "aaa"
}

func main(){
	m := middle{stu{"xiaoming"}}
	fmt.Println(m.getName())  //对外表现为重写
	fmt.Println(m.stu.getName()) //如果省略内嵌结构体名字，则调用的是改写的方法名。
}
```

### 设计模式中的原则

- 单一职责原则：就一个类而言，应该仅有一个引起它变化的原因。

  我们在做编程的时候，很自然的就会给一个类添加各种各样的功能。如果一个类承担的职责过多，就等于把这些职责偶合在一起，一个职责变化可能会削弱或抑制这个类完成其他职责的能力。这个原则告诉我们类越简单越好，把职责相互分离。MaritinFowler曾在《重构》中称`Long Method`：如果一个方法过长，其实极有可能是有坏味道了。

- 开放-封闭原则：软件实体（类、模块、函数等等）应该可以扩展，但是不可修改。即对于扩展是开放的，对于修改是封闭的。

  设计的时候，时刻要考虑尽量让这个类足够好，写好了就不要去修改了，如果新需求来，我们增加一些类就完事了，原来的代码能不动就不动。但是绝对的封闭是不可能的，我们可以做到的就是在发生小变化时，就及早去想办法应对发生更大变化的可能。所以在我们最初编写代码的时候，假设变化不会发生，当变化发生时，我们就要立即采取行动，创建抽象来隔离以后发生的同类变化。查明可能发生的变化所等待的时间越长，要创建正确的抽象就越困难。

  当然，开发人员应该仅对程序中出现频繁变化的那部分抽象，拒绝不成熟的抽象和抽象本身一样重要。

- 依赖倒转原则：高层模块不应该依赖底层模块，两个都应该依赖抽象；抽象不应该依赖细节，细节应该依赖抽象。

  面向过程开发时，为了使得常用代码可以复用，一般会写成许多的库函数，在做项目时候去调用这些底层函数就可以了，这就是高层模块依赖底层模块。比如PC里如果CPU、内存、硬盘都需要依赖具体的主板，那么主板一坏，所有部件都没用了。如果不管高层模块还是底层模块，他们都依赖于抽象，具体一点就是接口或抽象类，因为接口是稳定的，那么任何一个更改都不用担心其他收到影响，这就使得无论高层模块还是底层模块都可以很容易地被复用。

  说白了，就是针对接口编程，不要对实现编程。

- 里氏代换原则：子类型必须能够替换掉他们的父类型。

  也就是说，在软件里面，把父类都替换成它的子类，程序的行为没有变化。正是因为有了这个原则，使得继承复用称为了可能。依赖倒转原则可以说是面向对象设计的标记，用哪种语言来编写程序不重要，如果编写时考虑的都是如何针对抽象编程而不是针对细节编程，即程序中所有的依赖关系都是终止于抽象类或者接口，那就是面向对象的设计，反之就是过程化的设计了。

- 迪米特法则：也叫最少知识原则。如果两个类不必彼此之间通信，那么这两个类就不应当发生直接的相互作用。如果其中一个类需要调用另一个类的某一个方法的话，可以通过第三者转发这个调用。

  迪米特法则首先强调的是在类的结构设计上，每一个类都应当尽量降低成员的访问权限，根本思想是强调了类之间的松耦合。

- 合成/聚合原则：优先使用合成/聚合，而不是类继承。

  这样有助于保持每个类被封装，并被集中在单个任务上。这样类和类继承层次会保持较小规模，并且不太可能增长为不可控制的庞然大物。因为继承是一种强耦合的结构，父类变，子类就必须要变，所以在用继承的时候，一定是在`is-a`的关系时在考虑使用，而不是任何时候都去用。



### 设计模式核心理念——隔离变化

封装变化点是面向对象的一种很重要的思维方式。



## 创建型模式

- #### [简单工厂模式（Simple Factory）](https://github.com/YRXING/go-design-pattern/tree/main/01-Simple_Factory)

- #### [工厂方法模式（Factory Method）](https://github.com/YRXING/go-design-pattern/tree/main/02-Factory_Method)

- #### [抽象工厂模式（Abstract Factory）](https://github.com/YRXING/go-design-pattern/tree/main/03-Abstract_Factory)

- #### [创建者模式（Builder）](https://github.com/YRXING/go-design-pattern/tree/main/04-Builder)

- #### [原型模式（Prototype）](https://github.com/YRXING/go-design-pattern/tree/main/05-Prototype)

- #### [单例模式（Singleton）](https://github.com/YRXING/go-design-pattern/tree/main/06-Singleton)

## 结构型模式

- #### [外观模式（Facade）](https://github.com/YRXING/go-design-pattern/tree/main/07-Facade)

- #### [适配器模式（Adapter）](https://github.com/YRXING/go-design-pattern/tree/main/08-Adapter)

- #### [代理模式（Proxy）](https://github.com/YRXING/go-design-pattern/tree/main/09-Proxy)

- #### [组合模式（Composite）](https://github.com/YRXING/go-design-pattern/tree/main/10-Composite)

- #### [享元模式（Flyweight）](https://github.com/YRXING/go-design-pattern/tree/main/11-Flyweight)

- #### [装饰模式（Decorator）](https://github.com/YRXING/go-design-pattern/tree/main/12-Decorator)

- #### [桥接模式（Bridge）](https://github.com/YRXING/go-design-pattern/tree/main/13-Bridge)

## 行为型模式

- #### [中介者模式（Mediator）](https://github.com/YRXING/go-design-pattern/tree/main/14-Mediator)

- #### [观察者模式（Observer）](https://github.com/YRXING/go-design-pattern/tree/main/15-Observer)

- #### [命令模式（Command）](https://github.com/YRXING/go-design-pattern/tree/main/16-Command)

- #### [迭代器模式（Iterator）](https://github.com/YRXING/go-design-pattern/tree/main/17-Iterator)

- #### [模版方法模式（Template Method）](https://github.com/YRXING/go-design-pattern/tree/main/18-Template_Method)

- #### [策略模式（Strategy）](https://github.com/YRXING/go-design-pattern/tree/main/19-Strategy)

- #### [状态模式（State）](https://github.com/YRXING/go-design-pattern/tree/main/20-State)

- #### [备忘录模式（Memento）](https://github.com/YRXING/go-design-pattern/tree/main/21-Memento)

- #### [解释器模式（Interpreter）](https://github.com/YRXING/go-design-pattern/tree/main/22-Interpreter)

- #### [职责链模式（Chain of Responsibility）](https://github.com/YRXING/go-design-pattern/tree/main/23-Chain_of_Responsibility)

- #### [访问者模式（Visitor）](https://github.com/YRXING/go-design-pattern/tree/main/24-Visitor)

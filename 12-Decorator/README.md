### 装饰模式（Decorator）

动态的给一个对象添加一些额外的职责，就增加功能来说，装饰模式比生成子类更为灵活。

Go语言借助于匿名组合和非入侵式接口可以很方便的实现装饰模式。使用匿名组合，在装饰器中不必显示定义转调原对象的方法。

![image-20210824140538804](https://tva1.sinaimg.cn/large/008i3skNly1gtrufth2esj60ur0lamzl02.jpg)



```go
type Component interface {
	Operation()
}

type ConcreteComponent struct {}

func NewConcreteComponent() *ConcreteComponent  {
	return &ConcreteComponent{}
}

func (c *ConcreteComponent) Operation()  {
	fmt.Println("Concrete Component Operation")
}

type Decorator struct {
	component Component
}

func NewDecorator() *Decorator {
	return &Decorator{}
}

func (d *Decorator) Decorate(c Component) {
	d.component = c
}

func (d *Decorator) Operation()  {
	//实际执行的是Component的Operation()
	if d.component != nil {
		d.component.Operation()
	}
}

type ConcreteDecoratorA struct {
	*Decorator
	addedState string //本类独有功能
}

func NewConcreteDecoratorA() *ConcreteDecoratorA  {
	return &ConcreteDecoratorA{
		NewDecorator(),
		"",
	}
}
func (ca *ConcreteDecoratorA) Operation()  {
	ca.Decorator.Operation()
	ca.addedState = "New State"
	fmt.Println("Concrete Decorator A Operation")
}

type ConcreteDecoratorB struct {
	*Decorator
}

func NewConcreteDecoratorB() *ConcreteDecoratorB {
	return &ConcreteDecoratorB{
		NewDecorator(),
	}
}
func (cb *ConcreteDecoratorB) Operation(){
	cb.Decorator.Operation()
	cb.addedBehavior()
	fmt.Println("Concrete Decorator B Operation")
}

func (cb *ConcreteDecoratorB) addedBehavior()  {}

func main(){
	c := NewConcreteComponent()
	ca := NewConcreteDecoratorA()
	cb := NewConcreteDecoratorB()
	
	//一步步包装
	ca.Decorate(c)
	cb.Decorate(ca)
	cb.Operation()
}
```



装饰模式就是利用SetComponent（Decorate方法）来对对象进行包装的。

装饰模式是灵活的，如果只有一个ConcreteComponent类而没有抽象的Component类，那么Decorator类可以是ConcretComponent的一个子类。同样道理，如果只有一个ConcreteDecorator类，那么就没有必要建立一个单独的Decorator类，而可以把Decorator和ConcreteDecorator的责任合并成一个类。

核心理念就是Decorator里面含有一个component，然后可以通过Decorate进行包装（装饰的过程），然后改写Operation方法，增加独有的功能。



当系统需要新功能时，这些新加入的东西仅仅是为了满足一些只在某种特定情况下才会执行的特殊行为的需要，此时装饰模式提供了一个很好的解决方案。它把要装饰的功能放在单独的类中，并让这个类包装它所要装饰的对象。当需要执行特殊行为的时候，客户代码就可以在运行时根据需要有选择地、按顺序地使用装饰功能包装对象了。

Note：装饰模式的装饰顺序很重要，比如加密数据和过滤词汇都可以是数据持久化前的装饰功能，但顺序不对可能就会出问题。
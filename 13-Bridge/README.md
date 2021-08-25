### 桥接模式（Bridge）

将抽象部分与它的实现部分分离，使他们都可以独立地扩展。这里的实现指的是抽象类和它的派生类用来实现自己的对象。比如手机既可以按品牌来分类也可以按功能来分类，桥接模式的核心意图就是把这些实现独立处理啊，让他们各自地变化。

![image-20210824153618625](https://tva1.sinaimg.cn/large/008i3skNly1gtrx23i291j60jt0bj74y02.jpg)

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gtrx2e8ru1j60ip083t9302.jpg" alt="image-20210824153636387" style="zoom:107%;" />

桥接模式类似于策略模式，区别在于策略模式封装一系列算法使得算法可以互相替换。策略模式使抽象部分和实现部分分离，可以独立变化。

![image-20210824153344633](https://tva1.sinaimg.cn/large/008i3skNly1gtrwzf63k6j60oa0cz0tq02.jpg)

```go
//真正的实现接口
type Implementor interface {
	OperationImp()
}

type ConcreteImplementorA struct {}

func NewConcreteImplementorA() *ConcreteImplementorA{
	return &ConcreteImplementorA{}
}

func (c *ConcreteImplementorA) OperationImp()  {
	fmt.Println("Concrete Implementor A execution")
}

type ConcreteImplementorB struct {}

func NewConcreteImplementorB() *ConcreteImplementorB {
	return &ConcreteImplementorB{}
}
func (c *ConcreteImplementorB) OperationImp()  {
	fmt.Println("Concrete Implementor B execution")
}


//抽象接口
type Abstraction interface {
	Operation()
	SetImplementor(implementor Implementor)
}

type abstraction struct {
	implementor Implementor
}

func (a *abstraction) SetImplementor(implementor Implementor)  {
	a.implementor = implementor
}

func (a *abstraction) Operation()  {
	a.implementor.OperationImp()
}

type RefinedAbstraction struct {
	abstraction
}

func NewRefinedAbstraction() *RefinedAbstraction {
	return &RefinedAbstraction{}
}

func (r *RefinedAbstraction) Operation() {
	r.abstraction.Operation()
}

func main() {
	var ab Abstraction = NewRefinedAbstraction()
	ab.SetImplementor(NewConcreteImplementorA())
	ab.Operation()

	ab.SetImplementor(NewConcreteImplementorB())
	ab.Operation()
}
```



简单说就是将接口与其实现解耦，以便两者可以独立变化。

很多设计模式其实就是原则的应用而已，或许在不知不觉中就在使用设计模式了。
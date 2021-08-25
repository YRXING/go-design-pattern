### 策略模式（Strategy）

它定义了算法家族，分别封装起来，让他们之间可以互相替换，使得分离算法，符合开闭原则。此模式让算法的变化，不会影响到使用算法的客户。

![image-20210825112309846](https://tva1.sinaimg.cn/large/008i3skNly1gtsvd12f7xj60sn0dp40702.jpg)

```go
type Strategy interface {
	AlgorithmInterface()
}

type ConcreteStrategyA struct {}

func NewConcreteStrategyA() *ConcreteStrategyA {
	return &ConcreteStrategyA{}
}

func (ca *ConcreteStrategyA) AlgorithmInterface() {
	fmt.Println("algorithm A implementation")
}

type ConcreteStrategyB struct {}

func NewConcreteStrategyB() *ConcreteStrategyB  {
	return &ConcreteStrategyB{}
}

func (cb *ConcreteStrategyB) AlgorithmInterface() {
	fmt.Println("algorithm B implementation")
}

type Context struct {
	strategy Strategy
}

func NewContext(strategy Strategy) *Context {
	return &Context{
		strategy: strategy,
	}
}

func (ctx *Context) ContextInterface() {
	ctx.strategy.AlgorithmInterface()
}

func main() {
	ctx := NewContext(NewConcreteStrategyA())
	ctx.ContextInterface()

	ctx = NewContext(NewConcreteStrategyB())
	ctx.ContextInterface()
}
```



把变化的算法分离出来抽象出一个类，以便在Context中稳定的使用——`隔离变化`
### 状态模式（State）

当一个对象的内在状态改变时允许改变其行为，这个对象看起来像是改变了其类。

状态模式用于分离状态和行为。

状态模式主要解决的是当控制一个对象状态转换的条件表达式过于复杂时的情况。把状态的判断逻辑转移到表示不同状态的一系列类当中，可以把复杂的判断逻辑简化。

![image-20210825133743045](https://tva1.sinaimg.cn/large/008i3skNly1gtsz92p6qlj60rx0ibdhd02.jpg)



```go
type State interface {
	Handle(ctx *Context)
}

type ConcreteStateA struct {}

func NewConcreteStateA() *ConcreteStateA {
	return &ConcreteStateA{}
}

func (ca *ConcreteStateA) Handle(ctx *Context) {
	ctx.SetState(NewConcreteStateB())
}

type ConcreteStateB struct {}

func NewConcreteStateB() *ConcreteStateB {
	return &ConcreteStateB{}
}

func (cb *ConcreteStateB) Handle(ctx *Context)  {
	ctx.SetState(NewConcreteStateA())
}

type Context struct {
	state State
}

func NewContext(state State) *Context {
	return &Context{state: state}
}

func (ctx *Context) GetState() State {
	return ctx.state
}

func (ctx *Context) SetState(state State) {
	ctx.state = state
	fmt.Println("current state is ",reflect.TypeOf(state))
}

func (ctx *Context) Request(){
	ctx.state.Handle(ctx)
}

func main() {
	ctx := NewContext(NewConcreteStateA())

	ctx.Request()
	ctx.Request()
	ctx.Request()
	ctx.Request()
}
```

每次请求，都会更改context的状态，把状态分离出来，单独去更改context的状态。

状态模式的好处是将特定状态的行为都放入一个对象中，通过定义新的子类可以很容易地增加新的状态和转换。这样做的目的是为了消除庞大的条件分支语句，状态模式通过把各种状态转移逻辑分不到State的子类之间，来减少相互间的依赖。

当一个对象的行为取决于它的状态，并且它必须在运行时刻根据状态改变它的行为时，就可以考虑使用状态模式了。

状态模式和策略模式的UML类图很像，主要是因为都是隔离变化，策略模式是吧算法分离出来，而状态模式是把状态分离出来。另外状态模式隔离出来的状态还依赖于context，因为它要反过来影响对象的行为，而策略模式不需要。
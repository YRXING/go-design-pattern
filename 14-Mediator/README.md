### 中介者模式（Mediator）

用一个中介对象来封装一系列的对象交互。中介者使各对象不需要显示地相互引用，从而使其耦合松散，而且可以独立地改变他们之间的交互。

中介者模式封装对象之间互交，使依赖变的简单，并且使复杂互交简单化，封装在中介者中。

![image-20210824164851479](https://tva1.sinaimg.cn/large/008i3skNly1gtrz5kpegdj60t20fw0ur02.jpg)



```go
type Mediator interface {
	//定义一个抽象的发送消息方法，得到同事对象和发送消息
	Send(message string, colleague Colleague)
}

type ConcreteMediator struct {
	colleague1 *ConcreteColleague1
	colleague2 *ConcreteColleague2
}

func NewConcreteMediator() *ConcreteMediator {
	return &ConcreteMediator{}
}

func (cm *ConcreteMediator) Send(message string, colleague Colleague){
	if colleague == cm.colleague1 {
		cm.colleague2.Notify(message)
	}else {
		cm.colleague1.Notify(message)
	}
}

func (cm *ConcreteMediator) SetConcreteColleague1(colleague *ConcreteColleague1) {
	cm.colleague1 = colleague
}

func (cm *ConcreteMediator) SetConcreteColleague2(colleague *ConcreteColleague2) {
	cm.colleague2 = colleague
}

type Colleague interface {}

type colleague struct {
	mediator Mediator
}

func NewColleague(mediator Mediator) *colleague {
	return &colleague{mediator: mediator}
}

type ConcreteColleague1 struct {
	*colleague
}

func NewConcreteColleague1(mediator Mediator) *ConcreteColleague1 {
	return &ConcreteColleague1{NewColleague(mediator)}
}

func (c *ConcreteColleague1) Send(message string) {
	//使用中介者发送
	c.mediator.Send(message,c)
}

func (c *ConcreteColleague1) Notify(message string) {
	fmt.Println("colleague1 get message: ",message)
}

type ConcreteColleague2 struct {
	*colleague
}

func NewConcreteColleague2(mediator Mediator) *ConcreteColleague2 {
	return &ConcreteColleague2{NewColleague(mediator)}
}

func (c *ConcreteColleague2) Send(message string) {
	//使用中介者发送
	c.mediator.Send(message,c)
}

func (c *ConcreteColleague2) Notify(message string) {
	fmt.Println("colleague2 get message: ",message)
}

func main() {
	m := NewConcreteMediator()

	//让两个具体同时认识相同的中介者对象
	c1 := NewConcreteColleague1(m)
	c2 := NewConcreteColleague2(m)
	//让中介者认识各个具体同事类对象
	m.SetConcreteColleague1(c1)
	m.SetConcreteColleague2(c2)

	c1.Send("How are you?")
	c2.Send("I'm fine!")
}
```

如果不存在扩展情况，Mediator和ConcreteMediator可以合二为一。

当系统出现了多对多交互复杂的对象群时，不要急于使用中介者模式，而要先反思你的系统在设计上是不是合理。

中介者模式的优点就是减少了各个对象之间的耦合，可以站在更宏观的角度看待系统。而缺点就是控制集中化，把交互复杂行转变为了中介者的复杂性。
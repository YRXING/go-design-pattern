### 备忘录模式（Memento）

在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可将该对象恢复到原先保存的状态。

![image-20210825140743006](https://tva1.sinaimg.cn/large/008i3skNly1gtt048437vj60q60d10u202.jpg)

- Originator（发起人）：可根据需要决定Memento存储Originator的哪些内部状态。
- Memento（备忘录）：备忘录有两个接口，Caretaker只能看到备忘录的窄接口，它只能将备忘录传递给其他对象。Originator能够看到一个宽接口，允许它访问返回到先前状态的所有数据。
- Caretaker（管理者）：负责保存好备忘录，不能对备忘录的内容进行操作或检查。

```go
type Originator struct {
	//需要保存的属性，可能有多个
	state string
}

func NewOriginator() *Originator {
	return &Originator{}
}

func (o *Originator) GetState() string {
	return o.state
}

func (o *Originator) SetState(s string) {
	o.state = s
}

//将当前需要保存的信息导入并实例化出一个Memento对象
func (o *Originator) CreateMemento() *Memento {
	return NewMemento(o.state)
}

//恢复备忘录，将Memento导入并将相关数据恢复
func (o *Originator) SetStateFromMemento(memento *Memento)  {
	o.state = memento.GetState()
}

func (o *Originator) Show()  {
	fmt.Println("State: ",o.state)
}

type Memento struct {
	state string
}

func NewMemento(state string) *Memento {
	return &Memento{state: state}
}

func (m *Memento) GetState() string {
	return m.state
}

type Caretaker struct {
	memento *Memento
}

func NewCaretaker() *Caretaker  {
	return &Caretaker{}
}

func (c *Caretaker) GetMemento() *Memento {
	return c.memento
}

func (c *Caretaker) SetMemento(memento *Memento) {
	c.memento = memento
}


func main() {
	o := NewOriginator()
	o.SetState("On") //初始属性
	o.Show()

	c := NewCaretaker()
	c.SetMemento(o.CreateMemento())

	o.SetState("Off")
	o.Show()

	o.SetStateFromMemento(c.GetMemento()) //从备忘录恢复状态
	o.Show()
}
```



把要保存的细节封装在Memento中，哪一天要更改保存的细节也不用影响客户端了。

备忘录模式比较适用于功能比较复杂的，但需要维护或记录属性历史的类，或者需要保存的属性只是众多属性中的一小部分时，Originator可以根据保存的Memento信息还原到前一状态。

备忘录模式也是有缺点的：如果状态数据很大很多，那么在资源消耗上，备忘录对象会非常消耗内存。
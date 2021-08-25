### 观察者模式（Observer）

观察者模式又叫做`发布-订阅（Publish/Subscribe）模式`。它定义了一种一对多的依赖关系，让多个观察者对象同时监听某一个主题对象。这个主题对象在状态发生改变时会通知所有观察者对象，使他们能够自动更新自己。

![image-20210824192325341](https://tva1.sinaimg.cn/large/008i3skNly1gts3meq4qvj60o30gbgnk02.jpg)

```go
type Subject interface {
	Attach(observer Observer)
	Detach(observer Observer)
	Notify()
}

type subject struct {
	observers []Observer
}

func (s *subject) Attach(observer Observer)  {
	s.observers = append(s.observers,observer)
}

func (s *subject) Detach(observer Observer) {
	newObservers := []Observer{}
	for _,v := range s.observers {
		if v != observer {
			newObservers = append(newObservers,v)
		}
	}
	s.observers = newObservers
}

func (s *subject) Notify() {
	for _,v := range s.observers {
		v.Update()
	}
}

type ConcreteSubject struct {
	*subject
	subjectState string
}

func NewConcreteSubject() *ConcreteSubject {
	cs := &ConcreteSubject{
		&subject{make([]Observer,0)},
		"",
	}
	return cs
}

func (c *ConcreteSubject) SetSubjectState(state string) {
	c.subjectState = state
}

func (c *ConcreteSubject) GetSubjectState() string {
	return c.subjectState
}

type Observer interface {
	Update()
}

type ConcreteObserver struct {
	name string
	observerState string
	subject *ConcreteSubject
}

func NewConcreteObserver(subject *ConcreteSubject, name string) *ConcreteObserver {
	return &ConcreteObserver{
		name: name,
		subject: subject,
	}
}

func (co *ConcreteObserver) Update(){
	co.observerState = co.subject.subjectState
	fmt.Printf("observer %s's new state is %s\n",co.name,co.observerState)
}

func main() {
	s := NewConcreteSubject()
	s.Attach(NewConcreteObserver(s,"X"))
	s.Attach(NewConcreteObserver(s,"Y"))

	s.subjectState = "ABC"
	s.Notify()
}
```



当一个对象改变需要同时改变其他对象的时候，适合观察者模式。如果不考虑复用，可以直接定义Subject结构体和Oberver结构体。


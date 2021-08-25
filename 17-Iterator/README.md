### 迭代器模式（Iterator）

提供一种方法顺序访问一个聚合对象中各个元素，而又不暴露该对象的内部表示。也就是说，为遍历不同的聚集结构提供如开始、下一个、是否结束、当前哪一项等统一接口。

![image-20210824204926406](https://tva1.sinaimg.cn/large/008i3skNly1gts63xjo26j60m70fiq4502.jpg)

```go
type Iterator interface {
	First() Object
	Next() Object
	IsDone() bool
	CurrentItem() Object
}

type ConcreteIterator struct {
	aggregate *ConcreteAggregate
	current int
}

func NewConcreteIterator(aggregate *ConcreteAggregate) *ConcreteIterator {
	return &ConcreteIterator{
		aggregate: aggregate,
		current: 0,
	}
}

func (ci *ConcreteIterator) First() Object {
	return ci.aggregate.items[0]
}

func (ci *ConcreteIterator) Next() Object {
	var ret Object
	ci.current++
	if ci.current < ci.aggregate.GetCount() {
		ret = ci.aggregate.items[ci.current]
	}
	return ret
}

func (ci *ConcreteIterator) IsDone() bool {
	if ci.current >= ci.aggregate.GetCount() {
		return true
	}else {
		return false
	}
	
}

func (ci *ConcreteIterator) CurrentItem() Object  {
	return ci.aggregate.items[ci.current]
}

type Aggregate interface {
	CreateIterator() Iterator
}

type ConcreteAggregate struct {
	items []Object
	count int
}

func NewConcreteAggregate() *ConcreteAggregate {
	c := &ConcreteAggregate{}
	c.items = make([]Object,0)
	return c
}

func (ca *ConcreteAggregate) CreateIterator() Iterator {
	return NewConcreteIterator(ca)
}

func (ca *ConcreteAggregate) GetCount() int {
	return len(ca.items)
}

type Object interface {}
```



实际上，迭代模式就是把聚集对象交给别人，让别人代替自己去遍历。这样就把集合对象的遍历行为分离出来，抽象出一个迭代器类来负责，这样既可以做到不暴露集合的内部结构，又可以让外部代码透明的访问集合内部的数据。`这就是设计模式的核心理念——隔离变化。`

之所以定义抽象的Iterator，是因为我们可以根据不同的情景自定义不同的迭代模式，比如从后往前，而客户端感知不到底层的变化，统一使用。

当你需要对聚集对象又多种方式遍历时，可以考虑用迭代器模式。
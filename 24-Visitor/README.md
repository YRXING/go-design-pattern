### 访问者模式（Visitor）

访问者模式可以给一系列对象透明的添加功能，并且把相关代码封装到一个类中。对象只要预留接口`Accept`则后期为对象添加功能的时候就不需要改动对象。它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。

![image-20210825172205787](https://tva1.sinaimg.cn/large/008i3skNly1gtt5qhicxhj60nx0o941102.jpg)

```go
type Visitor interface {
	VisitConcreteElementA(a *ConcreteElementA)
	VisitConcreteElementB(b *ConcreteElementB)
}

type ConcreteVisitor1 struct {}

func NewConcreteVisitor1() *ConcreteVisitor1  {
	return &ConcreteVisitor1{}
}

func (c *ConcreteVisitor1) VisitConcreteElementA(a *ConcreteElementA)  {
	fmt.Printf("%s is visited by %s\n",reflect.TypeOf(a),reflect.TypeOf(c))
}

func (c *ConcreteVisitor1) VisitConcreteElementB(b *ConcreteElementB)  {
	fmt.Printf("%s is visited by %s\n",reflect.TypeOf(b),reflect.TypeOf(c))
}

type ConcreteVisitor2 struct {}

func NewConcreteVisitor2() *ConcreteVisitor2  {
	return &ConcreteVisitor2{}
}

func (c *ConcreteVisitor2) VisitConcreteElementA(a *ConcreteElementA)  {
	fmt.Printf("%s is visited by %s\n",reflect.TypeOf(a),reflect.TypeOf(c))
}

func (c *ConcreteVisitor2) VisitConcreteElementB(b *ConcreteElementB)  {
	fmt.Printf("%s is visited by %s\n",reflect.TypeOf(b),reflect.TypeOf(c))
}

type Element interface {
	Accept(visitor Visitor)
}

type ConcreteElementA struct {}

func NewConcreteElementA() *ConcreteElementA {
	return &ConcreteElementA{}
}

func (ca *ConcreteElementA) Accept(visitor Visitor){
	visitor.VisitConcreteElementA(ca)
}

func (ca *ConcreteElementA) Operation()  {} //其他相关方法

type ConcreteElementB struct {}

func NewConcreteElementB() *ConcreteElementB {
	return &ConcreteElementB{}
}

func (cb *ConcreteElementB) Accept(visitor Visitor){
	visitor.VisitConcreteElementB(cb)
}

func (cb *ConcreteElementB) Operation()  {} //其他相关方法

type ObjectStructure struct {
	elements []Element
}

func NewObjectStructure() *ObjectStructure {
	return &ObjectStructure{
		make([]Element,0),
	}
}

func (o *ObjectStructure) Attach( element Element) {
	o.elements = append(o.elements,element)
}

func (o *ObjectStructure) Detach(element Element) {
	newElements := make([]Element,0)
	for _, v := range o.elements {
		if v != element {
			newElements = append(newElements,v)
		}
	}
	o.elements = newElements
}

func (o *ObjectStructure) Accept(visitor Visitor) {
	for _, e := range o.elements {
		e.Accept(visitor)
	}
}

func main() {
	o := NewObjectStructure()
	o.Attach(NewConcreteElementA())
	o.Attach(NewConcreteElementB())

	v1 := NewConcreteVisitor1()
	v2 := NewConcreteVisitor2()

	o.Accept(v1)
	o.Accept(v2)
}
```



访问者模式的目的就是要把处理从数据结构中分离出来，如果系统有比较稳定的数据结构，又有易于变化的算法的话，使用访问者模式就是比较合适的，因为访问者模式使得算法操作的增加变得容易，增加新的操作意味着增加一个新的访问者。

访问者模式的缺点其实就是使增加新的数据结构变得困难了，因为每增加一个新的数据结构，就需要在Visitor接口中增加一个方法，这不符合开放-封闭原则，也就是说访问者模式适用于数据结构相对稳定的系统。所以GoF四人中的一个作者说过“大多时候你并不需要访问者模式，但当一旦你需要访问者模式的时候，那就是真的需要它了”。
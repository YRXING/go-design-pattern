### 模版方法模式（Template Method）

定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。模版方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

模版方法模式使用继承机制，把通用步骤和通用方法放到父类中，把具体实现延迟到子类中实现。使得实现符合开闭原则。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gtstj45k99j60is092aav02.jpg" alt="image-20210825101950515" style="zoom:120%;" />

因为Golang不提供继承机制，需要使用匿名组合模拟实现继承。

此处需要注意：因为父类需要调用子类方法，所以子类需要匿名组合父类的同时，父类需要持有子类的引用。而在面向对象语言中，通过在子类中重写方法即可实现。

```go
type AbstractClass interface {
	//一些抽象行为，放到子类去执行
  PrimitiveOperation1()
	PrimitiveOperation2()
  
  //模版方法，给出逻辑骨架
	TemplateMethod()
}

type abstractClass struct {
	AbstractClass //父类需要持有子类的引用
}

func (a *abstractClass) TemplateMethod(){
	//这里调用的是传进来的子类的方法
	a.PrimitiveOperation1()
	a.PrimitiveOperation2()
}

type ConcreteClassA struct {
	abstractClass //匿名组合父类，实现继承
}

func NewConcreteClassA() *ConcreteClassA{
	c := &ConcreteClassA{}
	c.AbstractClass = c
	return c
}

func (ca *ConcreteClassA) PrimitiveOperation1(){
	fmt.Println("concrete class a func1 execute")
}

func (ca *ConcreteClassA) PrimitiveOperation2(){
	fmt.Println("concrete class a func2 execute")
}


func main() {
	var c AbstractClass = NewConcreteClassA()
	c.TemplateMethod()
}
```



模版方法模式是通过把不变行为搬移到超类，去除子类中的重复代码来体现它的优势。

有时候，我们会遇到由一系列步骤构成的过程需要执行，这个过程从高层次看是相同的，但有些步骤的实现可能不同。这时候，通常可以考虑模版方法模式。

`这依旧是隔离变化，把不变的行为搬到父类，可变的行为在子类中实现。`
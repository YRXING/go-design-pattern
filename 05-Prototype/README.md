### 原型模式（Prototype）

原型模式使对象能复制自身，并且暴露到接口中，使客户端面向接口编程时，不知道接口实际对象的情况下生成新的对象。

![image-20210823154903767](https://tva1.sinaimg.cn/large/008i3skNly1gtqrt63fizj60p30eh3zr02.jpg)

```go
type Prototype interface {
  Clone() Prototype
}

type ConcretePrototype1 struct{
  id string
}
func (c *ConcretePrototype1) Clone() Prototype {
  return ConcretePrototype1{id: c.id}
}

func main(){
  p1 := &ConcretePrototype1{id: "p1"}
  clone1 := p1.Clone()
}
```

因为克隆实在太常用了，所以原型模式在很多语言中都是提供了一个克隆接口，这个接口只有一个Clone()方法，只要实现了这个接口就完成了原型模式。

当然实现克隆接口时候要注意深复制和浅复制。
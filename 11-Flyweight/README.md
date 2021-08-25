### 享元模式（Flyweight）

运用共享技术有效地支持大量细粒度的对象。

享元模式从对象中剥离出不发生改变且多个实例需要的重复数据，独立出一个享元，使多个对象共享，从而节省内存以及减少对象数量。

![image-20210824111130644](https://tva1.sinaimg.cn/large/008i3skNly1gtrpekn3i3j60qj0f7tau02.jpg)



```go
type Flyweight interface {
	Operation(int)
}

type ConcreteFlyweight struct {}

func NewConcreteFlyweight() *ConcreteFlyweight  {
	return &ConcreteFlyweight{}
}
func (c *ConcreteFlyweight) Operation(extrinsicstate int)  {
	fmt.Println("Concete Flyweight:",extrinsicstate)
}

type UnsharedConcreteFlyweight struct {}

func (u *UnsharedConcreteFlyweight) Operation(extrinsicstate int)  {
	fmt.Println("Unshared Concrete Flyweight:",extrinsicstate)
}

type FlyweightFactory struct {
	flyweights map[string]Flyweight
}

func NewFlyweightFactory() *FlyweightFactory{
	f := &FlyweightFactory{make(map[string]Flyweight)}
	f.flyweights["X"] = NewConcreteFlyweight()
	f.flyweights["Y"] = NewConcreteFlyweight()
	f.flyweights["Z"] = NewConcreteFlyweight()
	return f
}

func (f *FlyweightFactory) GetFlyweight(key string) Flyweight{
	//根据客户端要求，获得已生成的实例
	flyweight := f.flyweights[key]
  //不存在时，新建
	if flyweight == nil {
		flyweight = NewConcreteFlyweight()
		f.flyweights[key] = flyweight
	}
	return flyweight
}
```



FlyweightFactory实际上不一定在初始化时就生成好对象，完全可以什么也不做，到需要时，再去判断对象是否为null来决定是否实例化。

UnsharedConcreteFlyweight的存在是为了解决不需要共享对象的问题。

在享元对象内部并且不会随环境改变而改变的共享部分称为享元对象的内部状态，而罪者环境改变而改变、不可以共享的状态就是外部状态。

如果一个应用程序使用了大量的对象，而大量的这些对象造成了很大的存储开销时就应该考虑使用，因为有了共享对象，实例总数就大大减少了。

享元模式更多的时候是一种底层的设计模式，比如Java中的字符串常量池，如果定义两个相同的字符串，这两个字符串引用的就是同一个对象。

享元模式非常好的解决了对象的开销问题。
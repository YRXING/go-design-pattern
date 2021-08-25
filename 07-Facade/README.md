### 外观模式（Facade）

为子系统中的一组接口提供一个一致的界面，此模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。

![image-20210823172051870](https://tva1.sinaimg.cn/large/008i3skNly1gtqugkmrivj60si0i8myw02.jpg)



```go
type SubSystemOne struct {
  //MethodOne()
}

type SubSystemtwo struct {
  //MethodTwo()
}

type SubSystemthree struct {
  //MethodThree()
}

type Facade struct {
  one SubSystemOne
  two SubSystemTwo
  three SubSystemThree
}

func NewFacade() *Facade{
  return &Facade{
    //初始化成员
  }
}

func (f *Facade) MethodA(){
  fmt.Println("方法组A()...")
  f.one.MethodOne()
  f.two.MethodTwo()
  f.three.MethodThree()
}
func (f *Facade) MethodB(){
  fmt.Println("方法组B()...")
  f.two.MethodTwo()
  f.three.MethodThree()
}

```

外观模式很像数据库中的视图，为更好的使用子系统，提供一个接口供上层访问。

什么时候使用外观模式呢？

- 设计初期阶段：应该要有意识的将不同的两个层分离，比如经典的三层架构，层与层之间建立外观Facade，使得层与层之间的耦合度降低。
- 开发阶段：子系统往往因为不断重构演化而变得越来越复杂，增加外观Facade可以提供一个简单的接口，减少依赖。
- 维护一个遗留的大型系统时：新系统又需要老系统的功能，此时可以增加Facade，使新系统通过Facade接口与老系统交互，减少不必要的麻烦。
### 工厂方法模式（Factory Method）

简单工厂模式最大的优点在于工厂类包含了必要的逻辑判断，根据客户端的选择条件动态实例化相关的类。

工厂方法模式定义一个用于创建对象的接口，让子类决定实例化哪一个类，工厂方法使一个类的实例化延迟到子类。工厂方法把简单工厂的内部逻辑判断移到了客户端代码来进行，如果想要扩展，本来是改工厂类，现在需要修改客户端。

工厂其实就是创建实例的地方，简单工厂模式下，工厂是一个类，可以创建不同的实例。工厂方法模式下，工厂是一个接口，不同的子工厂实现创建实例的接口，用来创建特定的实例。

```go
//由结构体变为接口，实例化延迟到实现接口的结构体
type IFactory interface{
  NewOperator() Operator
}

type AddFactory struct{}

func (add *AddFactory) NewOperator() Operator {
  return &OperatorAdd{}
}

type SubFactory struct{}


func main() {
  iFactory := &AddFactory
  oper := iFactory.NewOperator()
  
  oper.SetA(1)
  oper.SetB(2)
  result := oper.Result()
}
```


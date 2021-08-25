### 简单工厂模式（Simple Factory）

就是有一个单独的类有一个静态方法，这个方法可以根据条件去创造不同实例的过程，这就是工厂。

Go语言没有构造函数这个概念，一般都是通过NewXXX()函数来实例化结构体对象。

Go语言没有静态的概念（没有静态变量、方法、类），所以静态类的方法一般直接定义，直接使用。

Go语言只能通过内嵌结构体来模拟继承父类的公共方法，而接口不能有成员变量和已经实现的方法，因此Operator相同的成员和方法只能单独抽象出一个公共结构体。

```go
type Operator interface {
	SetA(int)
	SetB(int)
	Result() int
}

//实现SetA、SetB方法
type OperatorBase struct {
  a int
  b int
}


//分别实现Result方法即可
type OperatorAdd struct {
  *OperatorBase
}

type OperatorSub struct {
	*OperatorBase
}


//直接定义一个OperatorFacotry的结构体是没有必要的，可以直接定义一个NewOperator的方法即可。
type OperatorFactory struct {}

func (f *OperatorFactory) NewOperator(operate string) Operator{
  switch operate {
    case "Add":
    	return &OperationAdd{}
    case "Sub":
   		return &OperationSub{}
  }
}

//客户端
func main(){
  operatorFactory := &OperatorFactory{}
  oper := operatorFactory.NewOperator("Add")
  oper.SetA(1)
  oper.SetB(2)
  result := oper.Result()
}
```


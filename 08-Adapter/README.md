### 适配器模式（Adapter）

将一个类的接口转换成客户希望的另外一个接口。Adapter模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。

在软件开发中系统的数据和行为都正确，但接口不符时，应该考虑用适配器。适配器模式主要应用于希望复用一些现存的类，但是接口又与复用环境要求不一致的情况。

![image-20210823181215985](https://tva1.sinaimg.cn/large/008i3skNly1gtqvy1s64uj60tk0d6wfn02.jpg)



```go
type Target interface{
  Request() string
}

//需要适配的类
type Adaptee struct{
  //SpecificRequest() string
}

type Adapter struct{
  adaptee Adaptee
}

func (a *Adapter) Request(){
  adaptee.SpecificRequest()
}
func NewAdapter() *Adapter{
  return &Adapter{//...}
}
  
func main(){
  target := NewAdapter()
  target.Request()
}

```



在双方都不太容易修改的时候使用适配器模式适配。
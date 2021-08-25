### 代理模式（Proxy）

为其他对象提供一种代理以控制对这个对象的访问。

![image-20210823183259157](https://tva1.sinaimg.cn/large/008i3skNly1gtqwjmo0knj60vq0h3tah02.jpg)

```go
type Subject interface{
  Request()
}

type RealSubject struct{}
func (r *RealSubject) Request(){
  fmt.Println("real request")
}

type Proxy struct{
  realSubject RealSubject
}
func (p *Proxy) Request(){
  //调用真是对象之前进行一系列的检查工作：检查缓存、判断权限、实例化真实对象等
  if realSubject == nil {
    realSubject = &RealSubject{}
  }
  resulSubject.Request
}

func main(){
  proxy := &Proxy{}
  proxy.Request()
}
```

如果RealSubject不继承Subject的话，就退化成了适配器模式了，所以两者有相似之处。

代理模式应用场景：

- 远程代理：为一个对象在不同的地址空间提供局部代表，这样可以隐藏一个对象存在于不同地址空间的事实。
- 虚拟代理：根据需要创建开销很大的对象，通过它来存放实例化需要很长时间的真实对象。
- 安全代理：用来控制真实对象访问时的权限。
- 智能指引：当调用真实的对象时，代理处理另外一些事。
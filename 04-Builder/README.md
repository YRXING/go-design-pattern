### 创建者模式（Builder）

将一个复杂的对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。如果使用了建造者模式，那么用户就只需要指定需要建造的类型就可以得到他们，而具体的建造过程和细节就不需要知道了。

创建者模式主要包含一个创建者接口，用于创建特定实例，实现该接口的不同创建者可以创建该实例的不同表示，然后通过Director，传入不同的创建者去创建相同类的不同实例。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gtqqm9eby3j60ld0dtaaz02.jpg" alt="image-20210823150755980" style="zoom:110%;" />

```go
type Product struct{
  //Add(part string)
  //Show()
}

//创建复杂对象各个部分的接口
type Builder interface{
  Part1()
  Part2()
  GetResult() Product
}
//Construct Product
type ConcreateBuilder1 struct{
  product Product
}
func (c *ConcreateBuilder1) Part1(){
  c.product.Add("head1")
}
func (c *ConcreateBuilder1) Part2(){
  c.product.Add("body1")
}
func (c *ConcreateBuilder1) GetResult(){
  return c.product
}

type ConcreateBuilder2 struct{}
//...


//构建一个使用Builder接口的对象
type Director struct{
  
}

func (d *Director) Construct(builder Builder){
  //指挥建造过程
  builder.Part1()
  builder.Part2()
}


func main(){
  director := &Director{}
  b1 := ConcreateBuilder1{}
  b2 := ConcreateBuilder2{}
  
  director.Construct(b1)
  p1 := b1.GetResult()
}
```

建造者模式的好处就是使得建造代码与表示代码分离，由于建造者隐藏了该产品是如何组装的，所以若需要一个产品的内部表示，只需要再定义一个具体的建造者就可以了。

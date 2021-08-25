### 组合模式（Composite）

将对象组合成树形结构以表示部分-整体的层次结构。组合模式使得用户对单个对象和组合对象的使用具有一致性。

![image-20210823185902749](https://tva1.sinaimg.cn/large/008i3skNgy1gtqxare8j0j60nh0h5abs02.jpg)

go语言通过‘接口+同名结构体默认实现+内嵌’模仿抽象类。

```go
type Component interface {
	//通常用Add和Remove方法来提供增加或移除树叶或树枝的功能
	Add(Component)
	Remove(Component)
	Display(int)
	//还可以增加Parent()、SetParent()等函数
}

type component struct {
	//需要继承的公共成员变量
	name string
}
func (c *component) Add(Component){}
func (c *component) Remove(Component){}
func (c *component) Display(depth int){
	for depth!=0 {
		fmt.Print("-")
		depth--
	}
	fmt.Println(c.name)
}

type Leaf struct {
	component
}
func NewLeaf(name string) *Leaf {
	return &Leaf{component{name: name}}
}

type Composite struct {
	component
	childs []Component
}
func NewComposite(name string) *Composite{
	return &Composite{
		component: component{name: name},
		childs: make([]Component,0),
	}
}
func (c *Composite) Add(child Component){
	c.childs = append(c.childs,child)
}
func (c *Composite) Remove(child Component){
	newChilds := make([]Component,0)
	for i,v := range c.childs {
		if c.childs[i] != v {
			newChilds = append(newChilds,v)
		}
	}
	c.childs = newChilds
}
func (c *Composite) Display(depth int){
	c.component.Display(depth)
	for _, child := range c.childs{
		child.Display(depth+2)
	}
}

func main(){
	root := NewComposite("root")
	root.Add(NewLeaf("Leaf A"))
	root.Add(NewLeaf("Leaf B"))

	comp := NewComposite("Composite X")
	comp.Add(NewLeaf("Leaf XA"))
	root.Add(comp)
	root.Display(1)
}
```



- 透明方式：上面Leaf实际意义不应该具备Add和Remove方法，但依旧实现，这种方式就是透明方式。这样做的好处是叶节点和枝接待您对于外界没有区别，他们具备完全一致的行为接口，这种接口实际是没有意义的。
- 安全方式：如果不希望做无用功，那么Component接口不声明Add和Remove方法，而是在Composite中声明所有用来管理子类对象的方法，这种就是安全方式。不过由于不够透明，所以树叶和树枝将具备不同的接口，客户端调用需要做出相应的判断。



当发现需求中是体现部分与整体层次的结构时，以及希望用户可以忽略组合对象与单个对象的不同，统一地使用组合结构中的所有对象时候，就应该考虑使用组合模式了。
### 职责链模式（Chain of Responsibility）

使多个对象都有机会处理请求，从而避免请求的发送者和接受者之间的耦合关系。将这个对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止。

![image-20210825161752971](https://tva1.sinaimg.cn/large/008i3skNly1gtt3vnoxx4j60qd0d10u202.jpg)



```go
type Handler interface {
	SetSuccessor(successor Handler)
	HandleRequest(request int)
}

type handler struct {
	successor Handler
}

func (h *handler) SetSuccessor(successor Handler) {
	h.successor = successor
}

func (h *handler) HandleRequest(request int)  {}

type ConcreteHandler1 struct {
	*handler
}

func (c *ConcreteHandler1) HandleRequest(request int){
	if request >= 0 && request < 10 {
		fmt.Printf("%s handle request %d\n",reflect.TypeOf(c),request)
	}else if c.successor != nil {
		c.successor.HandleRequest(request) //转移到下一位
	}
}

func NewConcreteHandler1() *ConcreteHandler1 {
	return &ConcreteHandler1{
		&handler{},
	}
}

type ConcreteHandler2 struct {
	*handler
}

func NewConcreteHandler2() *ConcreteHandler2 {
	return &ConcreteHandler2{
		&handler{},
	}
}

func (c *ConcreteHandler2) HandleRequest(request int){
	if request >= 10 && request < 20 {
		fmt.Printf("%s handle request %d\n",reflect.TypeOf(c),request)
	}else if c.successor != nil {
		c.successor.HandleRequest(request) //转移到下一位
	}
}

type ConcreteHandler3 struct {
	*handler
}

func NewConcreteHandler3() *ConcreteHandler3 {
	return &ConcreteHandler3{
		&handler{},
	}
}

func (c *ConcreteHandler3) HandleRequest(request int){
	if request >= 20 && request < 30 {
		fmt.Printf("%s handle request %d\n",reflect.TypeOf(c),request)
	}else if c.successor != nil {
		c.successor.HandleRequest(request) //转移到下一位
	}
}



func main() {
	var h1 Handler = NewConcreteHandler1()
	var h2 Handler = NewConcreteHandler2()
	var h3 Handler = NewConcreteHandler3()

	//设置职责链的上家与下家
	h1.SetSuccessor(h2)
	h2.SetSuccessor(h3)

	requests := []int{2,14,22,18}

	for _,v := range requests{
		h1.HandleRequest(v)
	}
}
```



这里发出这个请求的客户端并不知道当中哪一个对象最终处理这个请求，这样系统的更改可以在不影响客户端的情况下动态的重新组织和分配任务。

职责链模式有点类似于链表，successor是next指针，SetSuccessor为链表添加节点，请求沿着链表寻找可以处理该请求的节点。

职责链可简化对象的相互连接，他们仅需保持一个指向其后继的引用，而不需要保持它所有的候选接受者的引用。不过要当心，一个请求可能因为没有正确配置极有可能到了链的末端也都得不到处理。
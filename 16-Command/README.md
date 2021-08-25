### 命令模式（Command）

将一个请求封装为一个对象，从而使你可用不同的请求对客户进行参数优化；对请求排队或记录请求日志，以及支持可撤销的操作。

![image-20210824202308668](https://tva1.sinaimg.cn/large/008i3skNly1gts5cjqirej60mm0fiwfu02.jpg)

```go
type Command interface {
	Execute()
}

type ConcreteCommand struct {
	receiver *Receiver
}

func NewConcreteCommand(receiver *Receiver) *ConcreteCommand {
	return &ConcreteCommand{
		receiver: receiver,
	}
}

func (c *ConcreteCommand) Execute() {
	c.receiver.Action()
}

type Invoker struct {
	command Command
}

func NewInvoker() *Invoker {
	return &Invoker{}
}

func (i *Invoker) SetCommand(command Command)  {
	i.command = command
}

func (i *Invoker) ExecuteCommand()  {
	i.command.Execute()
}

type Receiver struct {}

func NewReceiver() *Receiver {
	return &Receiver{}
}
func (r *Receiver) Action(){
	fmt.Println("execute request!")
}

func main() {
	r := NewReceiver()
  //封装成一个命令
	c := NewConcreteCommand(r)
	i := NewInvoker()

	i.SetCommand(c)
	i.ExecuteCommand()
}
```



命令模式的本质是把某个对象的方法调用封装到对像中，方便传递、存储、调用。

命令模式作用于：

- 配置灵活
- 批处理
- 任务队列
- Undo、Redo

等把具体命令封装到对象中使用的场合。
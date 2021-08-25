### 解释器模式（Interpreter）

给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子。

解释器模式要解决的是如果一种特定类型的问题发生的频率足够高，那么可能就值得将该问题的各个实例表述为一个简单语言中的句子。这样就可以构建一个解释器，该解释器通过解释这些句子来解决该问题。

解释器模式的意义在于它分离多种复杂功能的实现，每个功能只需关注自身的解释。

![image-20210825152143621](https://tva1.sinaimg.cn/large/008i3skNly1gtt298bmduj60qi0gpmz402.jpg)

```go
type AbstractionExpression interface {
	Interpret(ctx *Context)
}

type TerminalExpression struct {}

func (t *TerminalExpression) Interpret(ctx *Context) {
	fmt.Println("terminal expression")
}

type NonterminalExpression struct {}

func (n *NonterminalExpression) Interpret(ctx *Context)  {
	fmt.Println("none terminal expression")
}

type Context struct {
	//省略get set方法，直接定义包外可见
	Input string
	Output string
}

func main() {
	ctx := &Context{}
	list := make([]AbstractionExpression,0)
	list = append(list,&TerminalExpression{})
	list = append(list,&NonterminalExpression{})

	for _ , exp := range list {
		exp.Interpret(ctx)
	}
}
```


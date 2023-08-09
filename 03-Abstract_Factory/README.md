### 抽象工厂模式（Abstract Factory）

提供一个创建一系列相关或相互依赖对象的接口，而无需指定他们具体的类。

例子：设计支持多种数据库的代码结构

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gtqpc7wiesj60tb0ofmzr02.jpg" alt="image-20210823142339559" style="zoom:80%;" />

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gtqp4qmx9jj60rr0ihmyu02.jpg" alt="image-20210823141629491" style="zoom:85%;" />

```go
type User struct{}
type Department struct{}

//数据访问接口
type IUser interface{
  Insert(user User)
  GetUser(id int)
}
type IDepartment interface {
  Insert(department Department)
  GetDepartment(id int)
}

type SqlserverUser struct{} //用于访问SqlServer的User
type AccessUser struct{} //用于访问Access的User

type SqlserverDepartment struct{}
type AccessDepartment struct{}

type IFactory interface{
  CreateUser() IUser
  CreateDepartment() IDepartment
}

//实现IFactory方法省略
type SqlserverFactory struct{} //创建Sqlserver数据库实例的工厂
type AccessFactory struct{} //创建Access数据库实例的工厂


func main(){
  user := User{}
  department := Department{}
  
  factory := AccessFactory{}
  iuser := factory.CreateUser()
  iuser.Insert(user)
}
```

抽象工厂模式很像是工厂方法模式的扩展，所生成的对象是有关联的，如果产品没有关联（不需要继承同一个接口），为每个产品的实例化都采用一个工厂的话，就退化为了工厂方法模式。

在抽象工厂模式中，具体工厂创建的是操作产品的接口，客户端是通过他们的抽象接口操纵实例，产品的具体类名也被具体工厂的实现分离，不会出现在客户端代码中，刚才的例子，客户端所认识的只有IUser和IDepartment，至于它是用SQL Server实现还是Access实现不重要。
在抽象工厂模式中，工厂和子工厂的结构和工厂方法模式一样，唯一的区别是创建的实例返回的是实例的接口，而实例可以有多种实现。

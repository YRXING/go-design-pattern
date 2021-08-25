### 单例模式（Singleton）

保证一个类仅有一个实例，并提供一个访问它的全局访问点。

通常我们可以让一个全局变量使得一个对象被访问，但它不能防止你实力化多个对象。一个最好的办法就是让类自身负责保存它的唯一实例，这个类可以保证没有其他实例可以被创建，并且它可以提供一个访问该实例的方法。

![image-20210823164135955](https://tva1.sinaimg.cn/large/008i3skNly1gtqtbpvtokj60t806a0th02.jpg)

go语言中没有静态方法，也没有构造方法，因此这些是通过全局变量+sync.Once来实现实例只被初始化一次的行为。

```go
//单例模式类，包私有
type singleton struct{}

var (
	instance *singleton
  once sync.Once
)

func GetInstance() singleton {
  once.Do(func() {
    instance = &singleton{}
  })
  return instance
}

```

而sync.Once是并发安全的，因此不用考虑多线程模式下的单例模式

```c++
class Singleton {
  private static Singleton instance;
  private Singleton(){}
  
  public static Singleton GetInstance(){
    //先判断实例是否存在，在加锁处理，即保证了线程安全，也保证了性能，这种做法称为双重锁定
    if(instance == null) {
      lock(syncRoot){
        //第二重instance判空为了防止多个线程冲破第一道障碍，虽然第一个线程锁定创建实例的操作，但外面还有多个线程等待着创建。
        if(instance == null){
          instance = new Singleton()
        }
      }
    }
    return instance;
  }
}
```

单例类分为饿汉式和懒汉式：

- 饿汉式：类一加载就实例化对象，所以要提前占用系统资源。
- 懒汉式：第一次被引用时，才会将自己实例化，会面临多线程访问的安全问题，需要双重锁定处理来保证线程安全，上面都是懒汉式。


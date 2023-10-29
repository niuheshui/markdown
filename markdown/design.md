# 设计模式([Design Patterns]([Design Patterns - Wikipedia](https://en.wikipedia.org/wiki/Design_Patterns#Notes)))

## 工厂

假设项目中的一个接口`BarInterface`。正常的想法可能`new`一个实现类。

```java
BarInterface b = new BarImplement();
```

但是当我们的需要修改使用的实现类时，就需要每个`new`都需要更改。

如果我们有一个工厂，使用工厂的静态方法用来返回实现类。

```java
public class BarFactory {
    public static BarInterface createObject() {
        return new BarImplement();
    }
}
```

当我们需要更改实现类时，只需更改工厂中的方法，提高了代码的可维护性。

工厂还可以实现缓存，例如`Integer.valueof()`，`List.Of()`。

## 单例

[一文带你搞定单例模式 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/426672787)
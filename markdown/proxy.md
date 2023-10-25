# Java动态代理

![image-20231025201128400](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20231025201128400.png)

运行结果

![image-20231025201556198](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20231025201556198.png)

`Proxy.newProxyInstance`中的这一行代码会创建一个代理类，也就是`$Proxy0`

![image-20231025201407274](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20231025201407274.png)

`$Proxy0`类的内容类似于

```java
public class $Proxy0 implements Hello {
    InvocationHandler handler;
    public $Proxy0(InvocationHandler handler) {
        this.handler = handler;
    }
    public void morning(String name) {
        handler.invoke(
           this,
           Hello.class.getMethod("morning", String.class),
           new Object[] { name });
    }
}
```

## 参考

[动态代理 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/1252599548343744/1264804593397984)

[你真的完全了解Java动态代理吗？看这篇就够了 - 简书 (jianshu.com)](https://www.jianshu.com/p/95970b089360)

[老大难的 Java ClassLoader 再不理解就老了 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/51374915)
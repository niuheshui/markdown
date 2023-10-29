# 数据库连接池

数据库为MySql。

## 连接数据库
与数据库建立连接本质上就是一次Tcp连接，经过Tcp的三次握手之后，进行MySql认证，此时就与数据库进行了连接。

## 执行Sql语句
将Sql语句发送到MySql服务器，执行返回结果。

## 关闭连接
Tcp四次挥手

## 使用连接池
使用连接池，就可以预先创建一些connection实例，在需要时从连接池中取出，不需要时放回。可以省去网络IO的耗时。

## 是否可以使用单例
使用连接池的目的是为了减少网络开销，创建单例connection对象也可以达到减少网络开销，但是在并发场景下，如果使用的数据库不能处理并发读写操作，可能会出现错误。  

还有一个区别体现在客户端，对于服务端来讲，发送的读写操作都是要执行的，但是客户端的多个实例对象在多线程下会比单例更快。单例在一个线程使用连接时其余线程只能等待。



## 参考
[关于Java: 数据库连接应该是单例的吗？](https://www.codenong.com/6507687/#:~:text=%E5%8D%95%E4%BE%8B%E5%8F%AA%E6%98%AF%E4%B8%BA%E5%85%B6%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%AE%9E%E4%BE%8B%E7%9A%84%E5%AF%B9%E8%B1%A1%E3%80%82%20%E5%AE%83%E4%B8%8E%E5%AF%B9%E8%B1%A1%E7%9A%84%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%E6%97%A0%E5%85%B3%E3%80%82%20%E5%A6%82%E6%9E%9C%E8%AF%A5%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%86%85%E9%83%A8%E6%B2%A1%E6%9C%89%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%EF%BC%8C%E5%88%99%E5%B0%86%E5%85%B6%E8%BD%AC%E6%8D%A2%E4%B8%BA%E5%8D%95%E4%BE%8B%E4%B8%8D%E4%BC%9A%E4%BD%BF%E5%85%B6%E5%85%B7%E6%9C%89%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%E6%80%A7%E3%80%82%20Connection%E5%AF%B9%E8%B1%A1%E4%B8%8D%E6%98%AF%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%E7%9A%84%EF%BC%8C%E5%9B%A0%E4%B8%BA%E6%82%A8%E4%B8%8D%E8%83%BD%E5%90%8C%E6%97%B6%E4%BB%8E%E5%A4%9A%E4%B8%AA%E7%BA%BF%E7%A8%8B%E4%B8%AD%E4%BD%BF%E7%94%A8%E5%AE%83%E3%80%82,%E5%A6%82%E6%9E%9C%E5%B0%86%E5%85%B6%E8%AE%BE%E7%BD%AE%E4%B8%BA%E5%8D%95%E4%BE%8B%EF%BC%8C%E9%82%A3%E4%B9%88%E5%A6%82%E6%9E%9C%E6%82%A8%E5%B0%9D%E8%AF%95%E4%BB%8E%E5%A4%9A%E4%B8%AA%E7%BA%BF%E7%A8%8B%E8%AE%BF%E9%97%AE%E8%AF%A5%E8%BF%9E%E6%8E%A5%EF%BC%8C%E5%88%99%E5%AE%B9%E6%98%93%E5%87%BA%E7%8E%B0%E4%B8%A5%E9%87%8D%E9%97%AE%E9%A2%98%E3%80%82%20%28%E6%82%A8%E5%BA%94%E8%AF%A5%E4%BD%BF%E7%94%A8%E7%9A%84%E6%98%AF%E8%BF%9E%E6%8E%A5%E6%B1%A0%20%28%E9%80%9A%E8%BF%87%E5%BA%93%E6%88%96ThreadLocal%29%EF%BC%8C%E4%BB%A5%E4%BD%BF%E6%AF%8F%E4%B8%AA%E7%BA%BF%E7%A8%8B%E5%8F%AA%E6%9C%89%E4%B8%80%E4%B8%AA%E8%BF%9E%E6%8E%A5%E3%80%82%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%9E%E6%8E%A5%E9%80%9A%E5%B8%B8%E4%B8%8D%E5%BA%94%E4%B8%BA%E5%8D%95%E4%BE%8B%E3%80%82)
[连接池与单例](https://blog.csdn.net/wansc12/article/details/129303390)
[为什么需要数据库连接池](https://zhuanlan.zhihu.com/p/396034724)

# 关于Java中的InputStream

## InputStream为什么慢

有下面这样一段`Java`代码。

```java
public static void readFile(File f) throws IOException {
    try (InputStream in = new FileInputStream(f)) {
        int n;
        while ((n = in.read()) != -1) 
            System.out.println(n);
    }
}
```

当我们调用`in.read()`时，程序发起`IO`系统调用，`read()`方法很懒，一次读取一个字节后之后就返回。


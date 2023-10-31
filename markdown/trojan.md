# Trojan代理服务器搭建

## 下载trojan-go

[p4gefau1t/trojan-go: Go实现的Trojan代理](https://github.com/p4gefau1t/trojan-go)

![image-20231031125956778](C:\Users\lenovo\Desktop\md\markdown\imgs\trojan\image-20231031125956778.png)

解压

![image-20231031130042984](C:\Users\lenovo\Desktop\md\markdown\imgs\trojan\image-20231031130042984.png)

## 自签证书

生成私钥

```bash
root@vultr:~/trojan# openssl ecparam -genkey -name prime256v1 -out ca.key
```



![image-20231031133506910](C:\Users\lenovo\Desktop\md\markdown\imgs\trojan\image-20231031133506910.png)

生成证书

```bash
root@vultr:~/trojan# openssl req -new -x509 -days 36500 -key ca.key -out ca.crt  -subj "/CN=niuheshui.com"
```



![image-20231031133536125](C:\Users\lenovo\Desktop\md\markdown\imgs\trojan\image-20231031133536125.png)



## 配置文件

![image-20231031130450766](C:\Users\lenovo\Desktop\md\markdown\imgs\trojan\image-20231031130450766.png)

```json
{
    "run_type": "server",
    "local_addr": "0.0.0.0",
    "local_port": 443,
    "remote_addr": "192.83.167.78",
    "remote_port": 80,
    "password": [
        "litengda123"
    ],
    "ssl": {
        "cert": "ca.crt",
        "key": "ca.key"
    }
}
```

![image-20231031135200242](C:\Users\lenovo\Desktop\md\markdown\imgs\trojan\image-20231031135200242.png)

## 开放端口

```bash
ufw allow 443
```



## 启动服务

```bash
root@vultr:~/trojan# nohup ./trojan-go > ./log 2>&1 &
```

![image-20231031141118285](C:\Users\lenovo\Desktop\md\markdown\imgs\trojan\image-20231031141118285.png)



## 客户端配置

客户端使用clash, 下面是clash配置

![image-20231031140935660](C:\Users\lenovo\Desktop\md\markdown\imgs\trojan\image-20231031140935660.png)

## 参考

[简介 - Trojan-Go Docs (p4gefau1t.github.io)](https://p4gefau1t.github.io/trojan-go/)****




# Vmess协议

## 安装v2ray



使用[官方安装脚本](https://github.com/v2fly/fhs-install-v2ray)进行安装

## tls签名

```bash
openssl ecparam -genkey -name prime256v1 -out server.key
```



```bash
openssl req -new -x509 -days 36500 -key server.key -out server.crt  -subj "/CN=niuheshui.com"
```

## v2ray配置

```bash
vim /etc/local/etc/v2ray/config.json
```



```json
{
  "inbounds": [
    {
      "port": 8388,
      "listen":"127.0.0.1",
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "af41686b-cb85-494a-a554-eeaa1514bca7",
            "alterId": 0
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "path": "/ray"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```

![image-20231031175500026](C:\Users\lenovo\Desktop\md\markdown\imgs\vmess\image-20231031175500026.png)

## nginx

安装



```bash
root@vultr:/usr/local/etc/v2ray# apt install nginx
```

nginx配置

```json
http {
  server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name 66.42.115.166;  #你的域名
    ssl_certificate       /usr/local/etc/v2ray/server.crt;
    ssl_certificate_key   /usr/local/etc/v2ray/server.key;
    ssl_session_timeout 1d;
    ssl_session_cache shared:MozSSL:10m;
    ssl_session_tickets off;

    ssl_protocols         TLSv1.2 TLSv1.3;
    ssl_ciphers           ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;

    location / {
      proxy_pass https://www.bing.com; #伪装网址
      proxy_ssl_server_name on;
      proxy_redirect off;
      sub_filter_once off;
      sub_filter "www.bing.com" $server_name;
      proxy_set_header Host "www.bing.com";
      proxy_set_header Referer $http_referer;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header User-Agent $http_user_agent;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto https;
      proxy_set_header Accept-Encoding "";
      proxy_set_header Accept-Language "zh-CN";
    }

    location /ray {
      proxy_redirect off;
      proxy_pass http://127.0.0.1:8388;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
  }

  server {
    listen 80;
    server_name 66.42.115.166;    #你的域名
    rewrite ^(.*)$ https://${server_name}$1 permanent;
  }
}

```

启动前记得开放端口

## 客户端配置文件

```yaml
mixed-port: 7890
allow-lan: true
bind-address: '*'
mode: rule
log-level: info
external-controller: '127.0.0.1:9090'
proxies:
  - name: "东京"
    type: ss
    # 节点服务器的 IP 或绑定的域名
    server: 167.179.65.149
    # Shadowsocks 服务所在端口
    port: 8388
    # 加密方式
    cipher: chacha20-ietf-poly1305
    # 密码
    password: "litengda123"
    # 是否开启 UDP
    udp: true
    plugin: v2ray-plugin
    plugin-opts: 
      mode: websocket
  - name: '新加坡'
    type: trojan
    server: 139.180.221.37
    port: 443
    password: litengda123
    udp: true
    sni: niuheshui.com
    skip-cert-verify: true
  - name: 芝加哥
    type: vmess
    server: 66.42.115.166
    port: 443
    uuid: af41686b-cb85-494a-a554-eeaa1514bca7
    alterId: 0
    cipher: auto
    tls: true
    udp: true
    skip-cert-verify: true
    sni: niuheshui.com
    network: ws
    ws-opts:
      path: /ray
```

## 参考

[节点搭建系列教程 - 科学上网 技术分享 (bulianglin.com)](https://bulianglin.com/archives/guide.html)
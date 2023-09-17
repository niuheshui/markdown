# 在`Ubuntu22.04`中安装`idea2023.2.2`

## 下载并安装`idea2023.2.2`

使用`wget`命令下载`wget https://download.jetbrains.com/idea/ideaIU-2023.2.2.tar.gz`. 

解压至`/opt`目录下`sudo tar -zxvf -C /opt && sudo mv /opt/ideaIU-2023.2.2 /opt/idea`.

## 添加桌面快捷方式

进入快捷方式配置目录`cd ~/.local/share/applications/`.

创建配置文件`vim idea.desktop`. 写入以下内容.

```ini
[Desktop Entry]
Name=IntelliJ IDEA
Comment=IntelliJ IDEA
Exec=/opt/idea/bin/idea.sh
Icon=/opt/idea/bin/idea.png
Terminal=false
Type=Application
Categories=Developer;
```

或者在`idea`设置中选择创建桌面条目.

## 配置`maven`

`idea`捆绑`maven`路径在`/opt/idea/plugins/maven/lib/maven3`.

编辑`$MAVEN_HOME/conf/settings.xml`文件配置[阿里远程仓库]([仓库服务 (aliyun.com)](https://developer.aliyun.com/mvn/guide)).

在`mirrors`标签中添加如下内容.

```xml
<mirror>                                                
    <id>alimaven</id>                                     
    <name>阿里云公共仓库</name>                           
    <url>https://maven.aliyun.com/repository/public</url> 
    <mirrorOf>central</mirrorOf>                          
</mirror>  
```



更改`idea`设置`maven`配置文件路径.

设置>构建执行部署>构建工具>`maven`.

取消勾选使用`.mvn/maven.conf`中的设置.

更改用户设置文件路径为`/opt/idea/plugins/maven/lib/maven3/conf/settings.xml`

## 安装插件

1. `Chinese(Simplified) Language Pack`
2. `CS 61B`
3. `IdeaVim`
4. `Java Visualizer`
5. `Maven Helper`
6. `MyBatisX`
7. `ptg`
8. `Translation`
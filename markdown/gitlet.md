# `CS61B Gitlet`



## 初始化

当前工作目录。

![image-20231023103923670](imgs\gitlet\image-20231023103923670.png)

初始化存储库，在工作目录下创建`.gitlet`存储库目录。

![image-20231023104246153](imgs\gitlet\image-20231023104246153.png)

其中包含第一次初始化提交。

## `add`



![image-20231023114334150](imgs\gitlet\image-20231023114334150.png)

添加文件，会向暂存区中添加文件名到`Blob`对象的映射。

![image-20231023120115858](imgs\gitlet\image-20231023120115858.png)

然后将暂存区对象序列化到`.gitlet/StagingArea`文件。

![image-20231023120319045](imgs\gitlet\image-20231023120319045.png)

此时`Blob`还没有真正添加到存储库。

### `Commit`

保存快照，会创建一个`Commit`对象。

![image-20231023120504806](imgs\gitlet\image-20231023120504806.png)

![image-20231023123040970](C:\Users\lenovo\Desktop\md\markdown\imgs\gitlet\image-20231023123040970.png)

修改`a.txt`，删除`b.txt`，添加`c.txt`，创建新快照。

![image-20231023174921502](imgs\gitlet\image-20231023174921502.png)

![image-20231023175423181](C:\Users\lenovo\Desktop\md\markdown\imgs\gitlet\image-20231023175423181.png)

存储库。

![image-20231023175805672](imgs\gitlet\image-20231023175805672.png)

## `branch`

![image-20231023203953847](imgs\gitlet\image-20231023203953847.png)

创建新分支，指向当前`Commit`。

![image-20231023205223742](imgs\gitlet\image-20231023205223742.png)






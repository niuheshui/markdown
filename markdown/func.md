# 函数调用时发生了什么

假设我们目前有这样一段程序

```c
int function1(int arg1, int arg2) {
    // 局部变量
    ...
    function2(parameter1, parameter2);
    ...
    return res;
}
```

#### 当我们刚进入`function1()`函数时, 栈的内容

...

arg2

arg1

调用function1()函数下一条指令地址       <= (%esp)

#### 进入函数后会先创建栈帧

...

arg2

arg1

调用function1()函数下一条指令地址     

上一级函数的栈帧						<= (%esp), %(ebp)

#### 调用`function2()`之前

...

arg2

arg1

调用function1()函数下一条指令地址      

上一级函数的栈帧					<= %(ebp)

局部变量1

局部变量2

...

parameter2																				

parameter1							<= (%esp)  此时还没真正进入`function2()`函数

#### 调用`function2()`函数

...

arg2

arg1

调用function1()函数下一条指令地址      

上一级函数的栈帧					<= %(ebp)

局部变量1

局部变量2

...

parameter2																				

parameter1							

下一条指令地址                     <= (%esp)

#### `function2()`返回之前

...

arg2

arg1

调用function1()函数下一条指令地址      

上一级函数的栈帧					

局部变量1

局部变量2

...

parameter2																				

parameter1							

下一条指令地址                    

`function1()`的栈帧				<= (%ebp)

`function2()`创建的局部变量

...						<= (%esp)

#### `function2()`返回后

...

arg2

arg1

调用function1()函数下一条指令地址      

上一级函数的栈帧					<= (%ebp)

局部变量1

局部变量2

...												

parameter2																				

parameter1							<= (%esp)

下一条指令地址                    

`function1()`的栈帧				

`function2()`创建的局部变量

...					

#### 最后

最后只需将(%esp)的值加上参数个数*寄存器大小就可以恢复到调用`function2()`函数之前的状态了
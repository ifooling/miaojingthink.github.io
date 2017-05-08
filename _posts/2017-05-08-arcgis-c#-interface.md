---
layout: post
title: 接口的显示实现，隐式实现区别
date: 2017-05-08 11:18:56
category: "arcgis"
---

## 接口的显示实现，隐式实现区别

```c#
public  interface IOut()  
{  
    void Out();  
}  
  
//隐式实现  
  
public class OutClass:IOut  
{  
    public void Out()  
    {  
        Console.WriteLine("隐式实现接口");  
    }  
}  
  
//都可以调用Out方法  
IOut out = new OutClass();  
out.Out();  
  
OutClass outClass = new OutClass();  
outclass.Out();  
  
  
//显示实现接口  
  
public class OutClass:IOut  
{  
    public void IOut.Out()  
    {  
        Console.WriteLine("隐式实现接口");  
    }       
}  
  
//通过这种方式，Out方法只能通过接口来调用  
IOut out = new OutClass();  
out.Out();  
```

### 结论：

* 隐式实现：接口和类都可以访问

* 显示实现：只有接口可以访问


### 显示实现的好处：

* 隐藏代码的实现

* 调用者只能通过接口调用而不是底层类来访问
# 循环依赖问题

# 问题

模块a,b,c

```
a 依赖 b 依赖 c
      c 依赖 b
```

首先引入a模块

就会出现c模块中的依赖b返回是空的Object对象

这是一个先有鸡蛋和鸡的问题

绝交办法就是不要出现循环依赖

webpack应该是抛出错误,而不是给我返回一个空对象,是没有意义的

# 参考链接

http://www.jianshu.com/p/5b360f18612d

https://cnodejs.org/topic/4f16442ccae1f4aa27001045
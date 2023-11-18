# 初始化
在index.ts中，可以看到整个初始化过程。
主要包括：

- 初始化引擎Engine(./engine)
- 后端诊断标志(./flags)
- 注册平台(浏览器，nodejs)
- 建立算子Handler
- 注册优化器

## 初始化引擎
具体可以参考core-engine文档

## 后端诊断标志
比如是否进行DEBUG，等等。


## 注册平台(浏览器，nodejs)
主要是针对各个不同的平台，提供一些个性化设置，比如
fetch函数等等

## 建立算子Handler
主要是支持张量的链式操作？


## 注册优化器
目前平台支持的优化器包括：
- AdadeltaOptimizer
- AdagradOptimizer
- AdamOptimizer
- AdamaxOptimizer
- MomentumOptimizer
- RMSPropOptimizer
- SGDOptimizer

# 如何建立与后端的联系
后端提供的算子，主要包括：
- abs
- acos
- 
- 。。。

这些算子在core/ops目录下已经提供了代理实现。
这些代理对象为前端应用编程提供一致的，便捷的模型。
各个代理本身则通过获取当前的engine对象，将对应的算子操作转发到对应的后端完成。
在core/ops/operation.ts中，提供一个包装函数op，可以完成将前端的算子及运算参数传递给后端。这个包装函数内置一个命名空间，可以确保运算完成后可以清除内存
通过ENGINE.runKernel可以调用后端的对应算子。


# 序列化

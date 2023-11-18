# engine
引擎是对后端进行统一管理。
# 启动流程

目前平台提供的后端包括：
cpu: tfjs-backend-cpu
nodegl:tfjs-backend-nodegl
wasm:tfjs-backend-wasm
webgl:tfjs-backend-webgl
tfjs-backend-webgpu

我们以cpu为例子，来说明注册过程
在cpu模块引入时，会自动调用tfjs-core提供的engine对象提供的方法registerBackend，进行后端注册：
```
registerBackend('cpu', () => new MathBackendCPU(), 1 /* priority */);
```
实际上就是注册一个实例对象MathBackendCPU到后端注册仓库中：

其它几个后端也是采取类似的方式进行注册。

# 后端实现概略
目前每一个后端实现代码都是单独的模块。

每一个后端提供的实例对象，在实现上都需要遵循一定的规范：
如cpu后端实例对象MathBackendCPU，我们可以看到，其类定义如下：

```
class MathBackendCPU extends KernelBackend{
    ...
}
```
直接继承了基类KernelBackend。
KernelBackend由tfjs-core模块提供。

下面简单给出KernelBackend的定义：
```
class KernelBackend implements TensorStorage, Backend, BackendTimer {
    ...
}
```

# 引擎对象Engine

内核启动时，会初始化一个引擎对象Engine（在engine.ts)提供实例化（p1305）
一个引擎对象本质上就是为前端提供访问后端的统一入口。
core对Engine的定义如下：
```
class Engine implements TensorTracker, DataMover {

}
```
目前实例化对象为ENGINE，一些方法通过globals.ts输出，如registerBackend。

# 激活后端
一般系统可以注册多个后端到engine中，但是在当前只能有一个后端处于激活状态。
这可以通过engine的方法setBackend(backendName)来指定。
激活时最主要的具体工作就是完成对该后端所有的kernel对象的注册工作。
后端所定义的所有kernel在加载时会自动注册到全局的缓存中（通过global_util完成，并按照不同的后端进行组织），因此我们可以通过这个全部缓存，加载所需要的kernel信息。

注册到全局缓存的kernel实际上是kernel的配置信息，以abs为例子：
```
{
  kernelName: Abs,
  backendName: 'cpu',
  kernelFunc: abs as unknown as KernelFunc,
}
```
注册信息包括：
kernel名称
对应后端名称
可以调用的函数

所谓激活，就是调用kernel.setupFunc(this.backendInstance)

# 

# 关于

提供高级接口，便于创建学习模型，完成训练与推理。

# 接口概述
index.ts提供这样的接口信息。
## 回调相关
export {CallbackList, CustomCallback, CustomCallbackArgs, History} from './base_callbacks';
export {Callback, callbacks, EarlyStopping, EarlyStoppingCallbackArgs} from './callbacks';

## 拓扑相关
export {InputSpec, SymbolicTensor} from './engine/topology';

## 基础模型
ModelAndWeightsConfig, Sequential, SequentialArgs
LayerVariable
## 训练模型
LayersModel, ModelCompileArgs, ModelEvaluateArgs

## 数据集
ModelFitDatasetArgs

## 训练
ModelFitArgs
ClassWeight, ClassWeightMap

## 模型导入
input, loadLayersModel, model, registerCallbackConstructor, sequential


## RNN相关
GRUCellLayerArgs, GRULayerArgs, LSTMCellLayerArgs, LSTMLayerArgs, RNN, RNNLayerArgs, SimpleRNNCellLayerArgs, SimpleRNNLayerArgs

## 其它
constraints, initializers, layers, metrics, models, regularizers

# 模型如何定义
有两个构建方法：sequential、LayerModel
## 顺序模型
顺序模型是任何一层的输出是下一层的输入的模型，即模型拓扑是一个简单的层“堆栈”，没有分支或跳过。
这意味着传递给tf.Sequential模型的第一层应该有一个定义的输入形状。这意味着它应该接收一个inputShape或batchInputShape参数，或者对于某些类型的层（recurrent、Dense…），它应该接收到一个inputDim参数。

tf.model（）和tf.sequential（）之间的关键区别在于，tf.sequival（）不太通用，只支持线性层堆栈。model（）更通用，支持任意的层图（没有循环）。

在包layers中提供很多构建组件：

## 基本层组件
activation
dropout
embedding
flatten
permute
repeatVector
reshape
spatialDropout1d
## 高级激活组件
elu
leakyRelu
prelu
reLU
softmax
thresholdedReLU
## 卷积组件
conv
。。。
## 层操作组件
add
average
dot
concatenate
maximum
minimum
multiply
## 正则化组件

## 池化组件

## RNN组件

# 模型如何训练

# 模型如何推理

# 模块说明
## backend
提供与backend相关的功能，主要是需要访问backend，完成一些计算相关的功能。具体包括：

与后端关系

## engine
为顺序模型和层模型提供基础类服务
以及模型的编译、训练、推理等功能
### 层对象Layer
baseRandomLayer(随机数生成：生成函数需要与backend交互RandomSeed)
Container(有向无环图，是LayerModel)
InputLayer 

### 执行器Executor
*使用具体的提要值执行SymbolicTensor。
“SymbolicTensor”对象是TF.js计算图中的一个图层节点
。对象由源层和输入支持
“SymbolicTensor”指向源层。如果输入键存在于“feedDict”中，此方法使用
从“feedDict”，评估源层的“call（）”方法，，否则，对“execute（）”本身的递归调用。


### 计算图（topology）
Node
Layer

### 训练器train
重点梳理一下。
尤其是与backend如何交互的
另外就是模型导出的结构

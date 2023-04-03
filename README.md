# how_to_write_user_op
OneFlow User op 开发笔记

# 动手开发oneflow算子小结 
## 工具篇

- https://github.com/ccssu/how_to_write_user_op/blob/main/docs/OneFlow/utils.md

## 开发篇

### 前向
- https://github.com/ccssu/how_to_write_user_op/blob/main/docs/OneFlow/forward.md
- https://github.com/ccssu/how_to_write_user_op/blob/main/docs/OneFlow/OneFlow_CUDA_Elementwise.md
### 后向
- https://github.com/ccssu/how_to_write_user_op/blob/main/docs/OneFlow/backward.md
### 注意事项
注意事项 C++
```
c++函数在linux系统下编译之后会变成类似下面的样子：
_ZNK4Json5ValueixEPKc
在linux命令行使用c++filter:
(python3.8)  c++filt _ZN7oneflow7user_op13ArangeFunctorILNS_10DeviceTypeE2EN10half_float4halfEEclEPNS_2ep6StreamES4_S4_lPS4_
oneflow::user_op::ArangeFunctor<(oneflow::DeviceType)2, half_float::half>::operator()(oneflow::ep::Stream*, half_float::half, half_float::half, long, half_float::half*)
```

格式化代码
进入编译的 `build` 目录下
```
ninja of_format
```
注意事项 python 
## 测试篇
- https://github.com/ccssu/how_to_write_user_op/blob/main/docs/OneFlow/test.md
## 文档篇
- https://github.com/ccssu/how_to_write_user_op/blob/main/docs/OneFlow/document.md


## 资料篇

[【ElementwiseUnary 算子代码实现梳理】](https://github.com/Oneflow-Inc/OneTeam/issues/1975)
[【矩阵乘 matmul 系列算子实现逻辑的梳理】](https://github.com/Oneflow-Inc/OneTeam/issues/1973)
[ 【一次 cpp 上 bug 的复现、定位和分析调试笔记（关于 global mode）】](https://github.com/Oneflow-Inc/OneTeam/issues/1978)
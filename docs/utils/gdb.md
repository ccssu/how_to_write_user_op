
### GDB
> 这里介绍 OneFlow 中使用<font color=Blue> gdb  debug</font>

Cmake 时候使用 <font face="黑体" color=green size=5>-DCMAKE_BUILD_TYPE=Debug </font>


<details open>
<summary> oneflow Debug 方式编译示例</summary>

```shell
git clone git@github.com:Oneflow-Inc/oneflow.git && cd oneflow 

mkdir -p build && cd build

cmake .. -C ../cmake/caches/cn/cuda.cmake   -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda  \
   -DCUDNN_ROOT_DIR=/usr/local/cudnn -DCMAKE_GENERATOR=Ninja -DCMAKE_BUILD_TYPE=Debug \
   -DCMAKE_EXPORT_COMPILE_COMMANDS=1

ninja -j32
```
</details>

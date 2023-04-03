## 结合NVTX注释上手nsys
> NVTX是一种工具，允许开发人员使用自定义标记注释其代码，这些标记可以在像NVIDIA Nsight Systems（nsys）这样的性能分析工具中可视化。这些标记可以帮助开发人员了解其代码的性能特征，并确定优化的领域。

- nvtx 教程: https://nvtx.readthedocs.io/en/latest/index.html

- [#1054 OneFlow Eager Profile套路交流贴](https://github.com/Oneflow-Inc/OneTeam/issues/1054)

## Python Demo 
```python
import numpy as np
import cupy as cp
import nvtx

@nvtx.annotate("fft function", color="blue")
def fast_fft(input_array):
    with nvtx.annotate("Copy input array to GPU and CuPy", color="red"):
        gpu_array = cp.array(input_array)
    with nvtx.annotate("GPU FFT operation", color="yellow"):
        result = cp.fft.fft(gpu_array)
    with nvtx.annotate("Copy back to CPU and Numpy", color="green"):
        cpu_result = cp.asnumpy(result)
    return cpu_result

for i in range(5):
    print(fast_fft(np.random.random(10)))

```
启动指令: 
```Usage
nsys profile python3 demo.py
```
![image](https://user-images.githubusercontent.com/118866310/227755777-e300b1e1-65ee-4be8-b538-23872407496a.png)
[上图对应的 nsys文件 report1.zip](https://github.com/wearmheart/worker/files/11070796/report1.zip)


### C++ Demo 
```c++
#include <cuda_runtime.h>
#include "nvToolsExt.h"
#include <iostream>

// 定义向量加法的 CUDA 核函数
__global__ void vectorAdd(const float *A, float *C, int N) {
    int i = blockDim.x * blockIdx.x + threadIdx.x;
    if(i < N) {
        C[i] = A[i] + 1.0f;
    }
}

// 启动 CUDA 核函数
void launch_kernel(const float *A, float *C, int N) {
    nvtxRangePushA("_FUNCTION_"); // 开始记录 _FUNCTION_ 的时间戳
    int threadsPerBlock = 256;
    int blocksPerGrid = (N + threadsPerBlock - 1) / threadsPerBlock;
    for(int i = 0; i < 4; i++) {
        nvtxRangePushA("vectorAdd"); // 开始记录 vectorAdd 的时间戳
        vectorAdd<<<blocksPerGrid, threadsPerBlock>>>(A, C, N);
        nvtxRangePop(); // 结束记录 vectorAdd 的时间戳
    }
    nvtxRangePop(); // 结束记录 _FUNCTION_ 的时间戳
}

int main() {
    const int N = 100;
    float *A, *C;
    cudaMallocManaged(&A, N * sizeof(float));
    cudaMallocManaged(&C, N * sizeof(float));
    for(int i = 0; i < N; i++) {
        A[i] = static_cast<float>(i);
        C[i] = 0.0f;
    }
    std::cout << "Launching kernel..." << std::endl;
    launch_kernel(A, C, N);
    cudaFree(A);
    cudaFree(C);
    return 0;
}
// 完成程序
```
### Reference

-  [NVIDIA性能分析工具Nsight Systems/Compute 的使用介绍](https://www.bilibili.com/video/BV15P4y1R7VG/?vd_source=0a13fe290c31c25fa9f746838c074df4)
- [#1054 OneFlow Eager Profile套路交流贴](https://github.com/Oneflow-Inc/OneTeam/issues/1054)
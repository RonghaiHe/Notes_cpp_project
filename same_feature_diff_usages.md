> 参考代码：[Kimera-VIO][Kimera-VIO]
# 引入
对于 `main` 函数，一开始是读入参数，不同的传感器有不同的参数，像相机需要内参和畸变系数；IMU需要噪声和随机游走的方差，二者在参数上有差别，但是读入参数的功能相同，所以可以只写一个函数来实现，再通过覆写来代替，以下是参考代码的案例实现。
## 代码引入
在 `/examples/KimeraVIO.cpp` 里，通过 `VIO::VioParams vio_params(FLAGS_params_folder_path);` 读入外部文件的参数。
> 注：`FLAGS_params_folder_path` 是 `gflags` 库的使用方法，通过如下代码读取：
```C++
// string 是该变量的类型，然后是名称，值，注释
DEFINE_string(
    params_folder_path,
    "../params/Euroc",
    "Path to the folder containing the yaml files with the VIO parameters.");
```

`VioParams` 类的构造函数在 `/src/pipeline/Pipeline-definitions.cpp` 里定义，在 `/include/kimera-vio/pipeline/Pipeline-definitions.h` 里声明。
首先是参数文件路径的赋值：
- 可以只有1个或2个路径，此时文件的命名需要为默认值
- 要是有所有文件的路径，则直接赋值

然后是 `parseYAML("")`（只要有文件路径都会到这一步）


[Kimera-VIO]: https://github.com/MIT-SPARK/Kimera-VIO "Kimera-VIO"

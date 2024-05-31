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

进入该文件的 `parseYAML` 函数后，首先创建了 `YamlParser` 类，用来读取pipeline参数，然后到了这份笔记的重点：
```C++
parsePipelineParams(imu_params_filepath_, &imu_params_);
```

# 代码赏析
## 代码
```C++
template <class T>
inline void parsePipelineParams(const std::string& params_path,
                                T* pipeline_params) {
  CHECK_NOTNULL(pipeline_params);
  static_assert(std::is_base_of<PipelineParams, T>::value,
                "T must be a class that derives from PipelineParams.");
  // Read/define tracker params.
  if (params_path.empty()) {
    LOG(WARNING) << "No " << pipeline_params->name_
                 << " parameters specified, using default.";
    *pipeline_params = T();  // default params
  } else {
    VLOG(100) << "Using user-specified " << pipeline_params->name_
              << " parameters: " << params_path;
    pipeline_params->parseYAML(params_path);
  }
}
```

## 简介
代码传入参数文件路径 `params_path` 和参数pipeline（指针类型） `pipeline_params`：
1. 通过 `glog` 里的函数 `CHECK_NOTNULL` 确定是否指针是否为空
2. 通过 `static_assert` 确定指针类型是否继承自 `PipelineParams`
3. if语句判断参数路径是否为空，是则警告，否则通过 `VLOG` 记录下来并进入该指针对应的函数，即：`pipeline_params->parseYAML(params_path);`

## 分析
- 对于调用该函数的代码，`imu_params_filepath_` 是IMU参数文件的路径，`imu_params_` 是在 `/include/kimera-vio/pipeline/Pipeline-definitions.h` 里结构体 `VioParams` 定义的，类型为 `ImuParams`
-  `ImuParams` 是在 `/include/kimera-vio/imu-frontend/ImuFrontendParams.h` 中定义的结构体
-  

[Kimera-VIO]: https://github.com/MIT-SPARK/Kimera-VIO "Kimera-VIO"

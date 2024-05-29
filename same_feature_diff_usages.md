> 参考代码：[Kimera-VIO][Kimera-VIO]

对于 `main` 函数，一开始是读入参数，不同的传感器有不同的参数，像相机需要内参和畸变系数；IMU需要噪声和随机游走的方差，二者在参数上有差别，但是读入参数的功能相同，所以可以只写一个函数来实现，再通过覆写来代替，以下是参考代码的案例实现。


[Kimera-VIO]: https://github.com/MIT-SPARK/Kimera-VIO "Kimera-VIO"

# 引入
需要写一个代码，动态读入机器人的数量，并基于此接收对应的话题。

# 简单方法
在launch文件中多次该执行文件，创建多个节点分别接收话题，有点复杂。

# 类
- `copilot` 建议：创建一个类，通过数组存储类的对象，基于单个节点处理。但是代码写好后报错：
  ```bash
  required from here
  /usr/include/c++/9/ext/new_allocator.h:146:4: error: use of deleted function ‘TopicPairProcessor::TopicPairProcessor(TopicPairProcessor&&)’
  ```

- `gpt-4o` 建议：数组存储类的指针：
  ```C++
  std::vector<std::unique_ptr<TopicPairProcessor>> processors;
  processors.push_back(std::make_unique<TopicPairProcessor>()
  ```
  成功。

`gpt-4o`的解释如下：不支持移动构造函数，而向量在需要调整大小时可能会移动内部元素。直接的解决方法为：**添加移动构造函数和移动赋值运算符**，而使用指针更简单。

```C++
// gpt-4o给出的参考代码
// 移动构造函数
Robot(Robot&& other) noexcept 
    : name(std::move(other.name)), subscriber(std::move(other.subscriber)) {
    other.subscriber = ros::Subscriber(); // 将被移动对象的 subscriber 置为无效
}

// 移动赋值运算符
Robot& operator=(Robot&& other) noexcept {
    if (this != &other) {
        name = std::move(other.name);
        subscriber = std::move(other.subscriber);
        other.subscriber = ros::Subscriber(); // 将被移动对象的 subscriber 置为无效
    }
    return *this;
}

// 删除拷贝构造函数和拷贝赋值运算符以避免意外拷贝
Robot(const Robot&) = delete;
Robot& operator=(const Robot&) = delete;
```

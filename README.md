# Vct::Test 库 说明

## 为什么做这个库？
因为`GoogleTest`库本身仅支持头文件导入标准库，还没有标准库模块的版本。

如果你的项目是C++23，使用了标准库模块，那么无法使用`GoogleTest`，会报错，提示重复声明/定义。

所以制作本库，用于测试C++23代码。


## 基本介绍
- C++23
- 标准库模块
- 仿GoogleTest
- 代码不足500行

## 使用方式：

### 1. 创建测试源文件
参考：
```c++
// main.cpp
#include "test/M_TEST_MAIN.h"
```
注意：
- `M_TEST_MAIN.h` 包含测试需要的宏，且附带主函数
- `M_TEST.h` 仅包含测试需要的宏，不包含主函数

### 2. CMake中加入头文件和模块接口文件
参考：
```cmake
add_executable(TTest main.cpp)
target_sources(TTest PUBLIC
    FILE_SET ttest_modules TYPE CXX_MODULES FILES src/Test.ixx
)
target_include_directories(TTest PUBLIC src)
```

### 3. 编译运行
此时程序正常运行，虽然没有任何测试样例

### 4. 添加测试样例

本库用法和`GTest`非常相似，但是所有宏都增加了前缀`M_`.

建议可以先去简单了解下`GoogleTest`的使用，就能轻松明白本库的使用方式。<br>
（作者没时间写详细教程。。。）

参考代码：
```c++
// main.cpp
#include "test/M_TEST_MAIN.h"

M_TEST(Example, Expect_test){
    M_EXPECT_THROW( throw std::exception(), std::exception );
    M_EXPECT_NO_THROW( 1+1 );
    M_EXPECT_ANY_THROW( throw std::exception() );

    M_EXPECT_EQ(1, 1);
    M_EXPECT_NE(1, 2);
    M_EXPECT_LT(1, 2);
    M_EXPECT_LE(1, 2);
    M_EXPECT_LE(1, 1);
    M_EXPECT_GT(2, 1);
    M_EXPECT_GE(2, 1);
    M_EXPECT_GE(1, 1);
}

M_TEST(Example, Assert_test){
    M_ASSERT_THROW( throw std::exception(), std::exception );
    M_ASSERT_NO_THROW( 1+1 );
    M_ASSERT_ANY_THROW( throw std::exception() );

    M_ASSERT_EQ(1, 1);
    M_ASSERT_NE(1, 2);
    M_ASSERT_LT(1, 2);
    M_ASSERT_LE(1, 2);
    M_ASSERT_LE(1, 1);
    M_ASSERT_GT(2, 1);
    M_ASSERT_GE(2, 1);
    M_ASSERT_GE(1, 1);
}
```

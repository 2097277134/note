# ROS2调试时遇到的问题

## 1:ros与conda产生冲突:

​	体现于c++编译失败:

```bash
Starting >>> village_li
Starting >>> village_wang
--- stderr: village_wang
Traceback (most recent call last):
  File "/opt/ros/foxy/share/ament_cmake_core/cmake/core/package_xml_2_cmake.py", line 21, in <module>
    from catkin_pkg.package import parse_package_string
ModuleNotFoundError: No module named 'catkin_pkg'
CMake Error at /opt/ros/foxy/share/ament_cmake_core/cmake/core/ament_package_xml.cmake:94 (message):
  execute_process(/home/zfly/miniconda3/bin/python3
  /opt/ros/foxy/share/ament_cmake_core/cmake/core/package_xml_2_cmake.py
  /home/zfly/jhx/town_ws/src/village_wang/package.xml
  /home/zfly/jhx/town_ws/build/village_wang/ament_cmake_core/package.cmake)
  returned error code 1
Call Stack (most recent call first):
  /opt/ros/foxy/share/ament_cmake_core/cmake/core/ament_package_xml.cmake:49 (_ament_package_xml)
  /opt/ros/foxy/share/ament_lint_auto/cmake/ament_lint_auto_find_test_dependencies.cmake:31 (ament_package_xml)
  CMakeLists.txt:30 (ament_lint_auto_find_test_dependencies)


---

Failed   <<< village_wang [0.71s, exited with code 1]
Aborted  <<< village_li [0.82s]

Summary: 0 packages finished [0.98s]
  1 package failed: village_wang
  1 package aborted: village_li
  1 package had stderr output: village_wang
```

### 解决:

python缺包导致,conda初始化cnoda init后每次打开中断会默认进入base空间python会被ros调用导致缺包;ros2支持python3报错为缺包,ros1仅支持python2

#### ros2:

```bash
pip install catkin_pkg -i https://pypi.douban.com/simple
```



#### ros1:

##### 方法一：取消终端 base 环境

需要我们需要先退出 anaconda base 环境，有两种方法

###### **方法一**

直接用 

```bash
conda deactivate
```

 退出，下次想使用 anaconda 的时候，再执行 conda activate base 进入；

###### 方法二

通过 conda config --set auto_activate_base false 进行配置 ，这样每次打开终端的时候，就不会自动进入 anaconda base 环境；若想恢复，则执行 conda config --set auto_activate_base true 即可。

原因：安装了anaconda环境，设置默认进入base

###### 修改默认配置
```bash
conda config --set auto_activate_base false

###### 查看anaconda配置
```

```bash
conda config --describe
```

报错的话升级版本试试

#### 方法二：conda 创建虚拟环境运行 ros

当前 ROS 是只支持 Python2.7 的。Python3 的支持在 ROS 的计划中，简单说来就是要到2019年ROS的N版本才能完全支持Python3。

首先要了解为什么 ROS 不能支持 Python3. 对于纯的 Python 代码同时支持 Python3 和 Python2.7 是比较容易的，基本上 ROS 的代码也都是支持的。问题在于包含了 C++ 或者 C 的那部分 Python 代码。Python2.7 和 Python3 的 c module 代码相差很大。一次只能编译其中的一种版本。而且很多module 没有做好 Python3 的支持。在 Python3 环境下也无法编译。这就是 ROS 无法支持 Python3 的原因。目前 ROS 的核心包都是支持用 Python3 从源码编译的。但是官方并没有发布 Python3 的软件包。所以想要使用的话要自己编译。下面介绍两种使用 Python3 的方法。

使用Python3和Python2.7的混合环境

原理：使用 conda 创建一个 Python3 的环境。然后在这个环境中编译安装自己需要的软件包。在引用软件包的时候，如果没有对应的 Python3 的软件包，会自动的去 Python2.7 的环境里面找。这样很多软件包都是可以通用的。当然对于没有做好 Python3 支持的软件包也是没法用的。

创建虚拟环境

```bash
conda create -n ros python=2.7 anaconda
source activate ros
```

虚拟环境安装 ros 所需要的包

# 查看使用的虚拟环境下的 pip，不是的话重开终端
```bash
which pip
pip install catkin-tools rospkg pyyaml empy numpy
```

只要装了rospkg就一定能使用(import) ROS自带的包了,并且能调用ROS自带包里的所有东西

source 环境

```bash
source /opt/ros/kinetic/setup.bash
source /home/PATH_TO_YOUR_WORKSPACE/devel/setup.bash
```


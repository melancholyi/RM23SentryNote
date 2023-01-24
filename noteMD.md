# GIT
## GitHub remote
```
https://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@github.com/melancholyi/xxxx.git
```
## 删除本地仓库
```bash
find . -name ".git" | xargs rm -Rf
```

# **CPP学习**
## **智能指针**
智能指针具有三种，分别为```shared_ptr, weak_ptr, unique_ptr```,三者具有各自的作用，建议搜一下资料，查看一下三者区别。这里简单记录一下我的理解，[参考链接](https://zhuanlan.zhihu.com/p/365414133)
### **```shared_ptr```**
- 托管**动态分配**对象，即普通局部变量和全局变量不可交给其托管
- 多个shared_ptr可以通过赋值的方式托管同一个对象

### **```weak_ptr```**
- 解决shared_ptr相互引用时产生死锁问题(引用次数永远不会=0)
- 弱引用，将 **<shared_ptr>** 对象```sp```赋值给 **<weak_ptr>** 对象```wp```时，不会增加sp的引用次数
- 未重载->和*，不可直接访问对象，但具有lock()方法，```sp = wp.lock()```

### **```unique_ptr```**
- 独占所有权,up之间不可直接赋值或者复制构造
- unique_ptr对象包装一个原始指针，并负责其生命周期


# minipc安装ubuntu和环境配置记录
[参考链接](https://blog.csdn.net/weixin_42915934/article/details/115212636)
## 1 制作启动U盘
### 1.1 UltraISO
### 1.2 Ubuntu镜像下载
[清华开源镜像站](https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/)
## 
## 2 安装
- 密码:sentry
### 2.1 Boot
F2 然后设置U盘启动为首选项，F10退出
### 2.2 安装
- 语言选择english
- 选择最小安装
- 清楚磁盘并安装
- 时区选择上海
## 3 环境配置
修改密码
```bash
sudo passwd
```
更新apt-get
```bash
sudo apt-get update
```
安装ROS
```bash
fishros 一键安装
```
包管理安装
```bash
# 安装git
sudo apt install git
# 安装pip
sudo apt-get install python3-pip 
sudo pip install rosdepc
sudo rosdepc init
sudo rosdepc update
```
vscode远程连接方法  
**ssh服务器端**，也就是小电脑端
```bash
# ssh远程连接 服务器包安装
sudo apt install openssh-server

# 打开这个ssh配置文件
sudo gedit /etc/ssh/sshd_config

# ctrl+F 修改以下部分并进行设置：
Port 22
PermitRootLogin yes     # 源文件中为#注释起来的，需要打开注释，并修改为yes
PubkeyAuthentication yes # 源文件中为#注释起来的，需要打开注释，并修改为yes
```
ssh客户端，VSCODE自己电脑端
- 扩展， Remote-ssh安装
- ctrl + shift + p 选择Remote-SSH : Connect to Host
- 添加新的ssh,格式为 ssh -p 《portnum》 username@hostname
- 然后配置文件 .ssh/config
- 修改 HostName 的后面东西为小电脑的ip地址



# ROS2及其他工具安装方案
## 1 ROS2安装
```
wget http://fishros.com/install -O fishros && . fishros
```
## 2 rosdep  
rosdep是一个ros的包管理器，自动配置工具包依赖  
### 2.1 安装与使用
```bash
# 因为rosdep使用的国外网站进行更新，如果没有代理的话很难更新成功，所以可以按照以下步骤，使用国内小鱼这个人开发的一个 rosdepc工具

sudo pip install rosdepc

# 如果显示没有pip可以试试pip3。
sudo pip3 install rosdepc

# 如果pip3还没有
sudo apt-get install python3-pip 
sudo pip install rosdepc
```
[rosdepc安装方案连接](https://zhuanlan.zhihu.com/p/398754989)  
### 2.2 使用
```bash
# 初始化和更新
sudo rosdepc init
sudo rosdepc update
```
### 2.3 补全依赖
rosdep的主要用途是安装工作空间中ros包的依赖，首先切换到工作空间下，然后运行下述命令即可安装该工作空间的所有依赖：
```bash
rosdep install --from-paths src --ignore-src -r -y
``` 
这里可能会遇见ERROR：  
```bash
ERROR: the following packages/stacks could not have their rosdep keys resovlved to system dependencies:
rm_seruial_driver:Cannot locate rosdep definition for [serial_driver]
```    
原因和解决方案：
这个就是简单的找不到这几个包，可能没装全。
直接install一下，注意上面的分隔符是_,下面安装的包名字分隔符为-
```bash
sudo apt-get install ros-galactic-serial-driver
```
    

---

# linux 
- ls命令
```
ls -a  #list all file. include hiden file which begin with "."
ls- l  #list more detail msg : 权限、所有者、文件大小等信息
```
- cd命令
```
cd ~ #go home
cd .. # 返回上一级目录
cd ../.. #返回上两级目录
```
- pwd 
```
pwd -P # 查看软链接的实际路径 
```
- mkdir
```
mkdir -p /... 不存在则创建
```
- rm
```
rm -i *.log # delete all .log file
rm -rf test # 删除 test 子目录及子目录中所有档案删除，并且不用一一确认：
rm -- -f*   # 删除以 -f 开头的文件
```
- mv
```
mv test.log test1.txt       # test.log was renamed to test1.txt
mv -i log1.txt log2.txt     # ）将文件 file1 改名为 file2，如果 file2 已经存在，则询问是否覆盖
mv log1.txt log2.txt log3.txt /test3  # 将文件 log1.txt,log2.txt,log3.txt 移动到根的 test3 目录中
mv * ../                    # 移动当前文件夹下所有文件到上一级目录
```
- cp
```
cp a.txt b.txtr
```

# Conda 虚拟环境
- 创建环境
```
conda create --name <envirment-name> python=3.6
```
- list
```
conda env list
```
- remove
```
conda env remove -n envName
```


# ubuntu problem solution
- 1 连不上网
```
sudo nmcli networking off
sudo nmcli networking on
sudo service network-manager restart
```
- 2 扩展显示屏G掉  
**!!!注意事项**：不要在ubuntu情况下，直接拔掉HDMI线  
解决方案：**拔掉HDMI**，重新安装驱动  
![img](./noteSrc/Problem1-driver.png)
重启电脑，后设置
```
xrandr # 显示屏幕

xrandr --output <mainScreenName> --below <slaverScreenName> --auto      # 将扩展屏放到主屏幕的上面
xrandr --output <mainScreenName> --right-of  <slaverScreenName> --auto  # 将扩展屏放在主屏幕的左边，
xrandr --output <mainScreenName> --left-of  <slaverScreenName> --auto   # 将扩展屏放在主屏幕的右边，
xrandr --output <mainScreenName> --same-as <slaverScreenName> --auto    # 设置双屏幕显示

```
# 库安装方法记录
## 1 ceres-solver （RM23 v2.1.0）
- 安装依赖
```
sudo apt-get install liblapack-dev libsuitesparse-dev libcxsparse3 libgflags-dev libgoogle-glog-dev libgtest-dev
```
- 官方下载tag-zip格式，[ceres-solver链接](https://github.com/ceres-solver/ceres-solver/tags)
- 编译安装
```
unzip ceres-solver-2.1.0.zip

cd ceres-solver-2.1.0/
mkdir build
cd build/
cmake ..    //commend：这边可能会花费很长时间
make
sudo make install
```


# ROS2学习笔记
## 1 注意事项
- makefile只支持TAB，不支持四空格代替
```
makefile:2: *** missing separator.  Stop.
```
## 2 功能包和工作空间
- ros2 pkg ... 
```
create       Create a new ROS 2 package
executables  Output a list of package specific executables
list         Output a list of available packages
prefix       Output the prefix path of a package
xml          Output the XML of the package manifest or a specific tag
```
- ros2 pkg create
```
ros2 pkg create <pkg-name> --build-type {cmake,ament_cmake,ament_python} --dependencies <dependName>
```
- ros2 pkg executables
```
ros2 pkg executables <node_name>
// sush as : ros2 pkg executables turtlesim
```
- ros2 pkg list  
list all package
- ros2 pkg prefix 
```
ros2 pkg prefix <package-name>
//such as : ros2 pkg prefix turtlesim
```
- ros2 pkg xml 
```
ros2 pkg xml turtlesim
```
## 3 colcon 
编译工具
- only build one package
```
colcon build --package-select <package_name>
```
- 不编译测试单元
```
colcon build --package-select <package_name> --cmake-args -DBUILD_TESTING=0
```
- test
```
colcon test
```
- **允许通过更改src下的部分文件来改变install**
如此每次调整python脚本就不用重新build了
```
colcon build --symlink-install
```
---
**题外话，如何使用clion开发ros2：** [官方教程链接](https://www.jetbrains.com/help/clion/ros2-tutorial.html?keymap=secondary_macos#change-prj-root)  
还有以下一个注意事项，这个教程貌似是windows的配置教程，因此colcon build的时候，要修改为以下这个
```
colcon build --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
```
或者在package的CMakeLists.txt中添加一下下条命令
```cmake
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```
---
## 4 topic
topic无返回，适合于单向或者大量数据传递
### ros2 topic 
使用的demo
```bash
ros2 run demo_nodes_py listener
ros2 run demo_nodes_cpp talker 
```
ros2 topic
```
Commands:
  bw     Display bandwidth used by topic 显示topic带宽
  delay  Display delay of topic from timestamp in header 展示delay
  echo   Output messages from a topic 输出来自topic的msg
  find   Output a list of available topics of a given type 
  hz     Print the average publishing rate to screen 
  info   Print information about a topic 
  list   Output a list of available topics
  pub    Publish a message to a topic 
  type   Print a topic's type
```
- ros2 topic bw 
```
ros2 topic bw <topic_name>
```
- ros2 topic delay
```
ros2 topic delay <topic_name>
```
- ros2 topic list
- ros2 topic list -t （增加消息类型）
- ros2 topic info <topic_name>
```
ros2 topic info /chatter
melancholycy@super:~$ ros2 topic info /chatter
    Type: std_msgs/msg/String   //得到此topic的消息类型
    Publisher count: 1
    Subscription count: 1
```
- ros2 topic echo <topic_name>
- ros2 topic pub
```
ros2 topic pub /chatter std_msgs/msg/String 'data: "123"'
# 上面因为listener是py格式，因此发送时是字典形式
```
- ros2 topic info 
```
ros2 topic info /chatter
```
---
## 5 server
C/S 模型，也可称为 request / response 模型  
服务是双向的，client发送请求，server响应请求。
- ros2 service -h
```
Commands:
call  Call a service
find  Output a list of available services of a given type
list  Output a list of available services
type  Output a service's type
```
---
这里调用最简单的服务example
```
ros2 run examples_rclpy_minimal_service service
```
---
- ros2 service list  
列出现有服务和一些参数get和set方法，示例如下
```
melancholycy@super:~$ ros2 service list
/add_two_ints
/minimal_service/describe_parameters
/minimal_service/get_parameter_types
/minimal_service/get_parameters
/minimal_service/list_parameters
/minimal_service/set_parameters
/minimal_service/set_parameters_atomically
```
---

- ros2 service type <service_name>  
  查看服务接口类型,然后在可以调用他
```
ros2 service type /add_two_ints
such as :
melancholycy@super:~$ ros2 service type /add_two_ints 
example_interfaces/srv/AddTwoInts
```
---

- ros2 service call ...  
手动调用服务
```
ros2 service call /add_two_ints example_interfaces/srv/AddTwoInts "{a: 5,b: 50}"
```
---

- ros2 service find
作用与type相反，通过接口类型寻找服务名称
```
ros2 service find example_interfaces/srv/AddTwoInts
```

---

## 6 message
topic的publisher和subscriber使用msg通信，下面列出ROS2中自带和常见的message，用作记录和后续查阅  
常见message
- **时间相关**   
  ```ros2 interface show builtin_interfaces/msg/...```
- **基础数据类型**  
  ```
  ros2 interface show example_interfaces/msg/...
  ros2 interface show std_msgs/msg...
  ```
- **几何相关**  
  多用于机器人，具有以下定义好的接口信息:accel、inertia、point、polygon(多边形)、pose(position and orientation)、Quaternion(四元数)、transform(刚体变换，translation + rototion)、twist(线速度+角速度)  
  ```ros2 interface show geometry_msgs/msg/...```
- **传感器数据**  
  收集传感器类型数据，具有一下定义好的接口信息：BattaryState、CameraInfo、Image、Imu、JointState、Joy、LaserScan、PointCloud等等机器人领域常用传感器  
  ```ros2 interface show sensor_msgs/msg/...```
- **tf2数据类型**  
  tf2数据类型只有两个分别为：```TF2Error```和```TFMessage```


# ROS2 Humble code usage note
## 1 创建WS和node
### 1.1 创建WorkSpace和package
```shell
cd myCode/RM/
mkdir -p scu_sentry_ws
cd scu_sentry_ws
ros2 pkg create <package_name> --build-type ament_cmake --dependencies rclcpp
touch <package_name>/src/<cpp_name>.cpp
```
### 1.2 创建节点模板
```cpp
#include "rclcpp/rclcpp.hpp"

class TopicPublisher01 : public rclcpp::Node
{
public:
    // 构造函数,有一个参数为节点名称
    TopicPublisher01(std::string name) : Node(name)
    {
        RCLCPP_INFO(this->get_logger(), "大家好，我是%s.", name.c_str());
    }

private:
    // 声明节点
};

int main(int argc, char **argv)
{
    rclcpp::init(argc, argv);
    /*产生一个的节点*/
    auto node = std::make_shared<TopicPublisher01>("topic_publisher_01");
    /* 运行节点，并检测退出信号*/
    rclcpp::spin(node);
    rclcpp::shutdown();
    return 0;
}
```

### 1.3 修改CMakeLists.txt
```cmake
add_executable(<exe_name> src/<cpp_name>.cpp)
ament_target_dependencies(<exe_name> rclcpp)

install(TARGETS
        <exe_name>
  DESTINATION lib/${PROJECT_NAME}
)
```
### 1.4 colcon编译
```shell
cd myCode/RM/scu_sentry_ws/
colcon build --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
source install/setup.bash
```
### 1.5 clion打开
```
# 打开工程
file-->open-->scu_sentry_ws/build/compile_commands.json
# 修改文件主目录
Tools-->Compilation Database-->Change Project Root
```
### 1.6 rclcppNode API Usage
```cpp
#include "rclcpp/rclcpp.hpp"

//class
class myNode : public rclcpp::Node{
    explicit myNode(const std::string& nodename) :Node(name){...}
};

//int main spin node
int main(int argc, char** argv){
    rclcpp::init(argc,argv);
    auto node = std::make_shared<myNode>("myNodeName");
    rclcpp::spin();
    rclcpp::shutdown();
    return 0;
}
```

## 2 Basic Usage
### 2.1 Topic 
topic --- message 打交道
![img](./noteSrc/ROS2ComStructChart.png)
#### 2.1.1 导入消息接口message
- CmakeLists.txt 添加
```cmake
# 这里的std_msgs为内置消息类型，以下仅作为范例，然后其他需要的可以进行更换
find_package(std_msgs REQUIRED)
ament_target_dependencies(<cpp_name> rclcpp std_msgs)
```
- packages.xml
```xml
<depend> std_msgs </depend>
```
- <cpp_name>.cpp
```cpp
#include "std_msgs/msg/string.hpp"
```
- 重新编译
```
colcon build --packages-select <package_name> --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
```
#### 2.1.2 publisher class example
```cpp
class cTopicPublisher : public rclcpp::Node
{
public:
    // constructor, paramIn : Node_name
    explicit cTopicPublisher(const std::string& name) : Node(name){
        RCLCPP_INFO(this->get_logger(), "node:%s construct ok", name.c_str());
        //创建publisher<message type> 可以通过ros2 interface list查看标准msg type 也可以自定义
        mCmdPublisher = this->create_publisher<std_msgs::msg::String>("cmd",10); //publisher名字和qos服务质量
        //create timer param in : period and callbackFunctionName
        mTimer = this->create_wall_timer(std::chrono::milliseconds(500),std::bind(&cTopicPublisher::pubMsgCallback,this));
    }

    //private function
private:
    void pubMsgCallback(){
        std_msgs::msg::String msg;
        msg.data = "forward";
        RCLCPP_INFO(this->get_logger(),"publishing: '%s'",msg.data.c_str());
        mCmdPublisher->publish(msg);
    }
private:
    // publisher
    rclcpp::Publisher<std_msgs::msg::String>::SharedPtr mCmdPublisher;
    // timer --> control msg pub period
    rclcpp::TimerBase::SharedPtr mTimer;

};
```
#### 2.1.3 subscriber class example code
```cpp
class cTopicSubscriber : public rclcpp::Node{
    using subMsgType = std_msgs::msg::String;
public:
    explicit cTopicSubscriber(const std::string& name):Node(name){
        RCLCPP_INFO(this->get_logger(),"hello l am '%s'",name.c_str());
        //create subscriber
        mCmdSubscriber = this->create_subscription<subMsgType>("cmd",10,std::bind(&cTopicSubscriber::topicCallBack,this,std::placeholders::_1));
    }
private:
    void topicCallBack(const std_msgs::msg::String::SharedPtr  msg){
        RCLCPP_INFO(get_logger(),"received:[%s]",msg->data.c_str());
    }
private:
    //create subscriber <msg type>
    rclcpp::Subscription<std_msgs::msg::String>::SharedPtr mCmdSubscriber;
};
```
### 2.2 Service 
service -- interface 打交道  
![img](./noteSrc/ROS2ComStructChart.png)
#### 2.2.1 添加interface配置
- cmakelist
```cmake
# 1 find package 
find_package(<interface_pkg_name> REQUIRED)
# 2 添加依赖
ament_target_dependencies(<exe_name> rclcpp <interface_pkg_name>)

# such as :<interface_pkg_name> =  example_interfaces
```
- package.xml
```xml
<depend> service_pkg_name </depend>
```
#### 2.2.2 server code api
- 1 declaration server sharedptr
```cpp
//1 <interface type> 一般格式为 serviceType::srv::interfaceName
rclcpp::Service<serviceType>::SharedPtr mService;
// such as :
rclcpp::Service<example_interfaces::srv::AddTwoInts>::SharedPtr mService;
```
- 2 code handle service function
```cpp
//2 此处sunch as : serviceType <==> example_interfaces::srv::AddTwoInts
void handleFunName( 
      const std::shared_ptr<serviceType::Request > request,
            std::shared_ptr<serviceType::Response> response){
}
```
- 3 create server
```cpp
//3 此处sunch as : serviceType <==> example_interfaces::srv::AddTwoInts
mService = this->create_service<serviceType>(
        "srv_name",
        std::bind(&className::handleFunName,this,std::placeholders::_1,std::placeholders::_2)
        );
```
#### 2.2.3 client code api
以下such as : serviceType <==> example_interfaces::srv::AddTwoInts
- 1 statement client
```cpp
rclcpp::Client<serviceType>::sharedPtr mClient;
```
- 2 callback
```cpp
void callBack(rclcpp::Client<serviceType>::SharedFuture res){
    auto response = res.get();
    //repsonse->... 访问响应数据
}
```
- 3 wait for server
```cpp
while (!mClient->wait_for_service(std::chrono::seconds(1))){}
```
- 4 create request
```cpp
auto request = std::make_shared<serviceType_Request>();
// request->... = data 填充数据
```
- 5 异步发送并绑定callback
```c++
mClient->async_send_request(<requestValName>,std::bind(&className::callBackName,this,std::placeholders::_1...占位))
```
- 6 创建client
```c++
mClient = this->create_client<serviceType>("srv_name");
```
- 7 发送request
```c++
node->sendRequestFunName(type param, ....)
```
### 2.3 interface （custom)
创建自己的msg和srv格式消息
#### 2.3.1 create pkg
```shell
ros2 pkg create --build-type ament_cmake pkgname_interface --dependencies rosidl_default_generators 
```
#### 2.3.2 create msg and srv
```shell
mkdir srv
mkdir msg
touch srvName.srv
touch msgName.msg
# 编写即可
```
#### 2.3.3 config
**CMakeList.txt**
```cmake
# interface adding
find_package(.... REQUIRED)
rosidl_generate_interfaces(${PROJECT_NAME}
        "msg/msgName.msg"
        "srv/srvName.srv"
        DEPENDENCIES ......
        )
```
注意事项
- 以上内容必须在ament_package()之前
- 用了标准数据则需要一并注明，such as : find_package(geometry_msgs REQUIRED)
package.xml
- 一定记着增加依赖 **DEPENDENCIES** ... such as： DEPENDENCIES geometry_msgs

**package.xml**
```xml
<depend>geometry_msgs</depend>  
<depend> rosidl_default_generators</depend>
```
#### 2.3.4 colcon build
#### 2.3.5 创建node pkg
自定义的interface当作dependencies使用即可
```shell
ros2 pkg create example_robot_topicandsrv --build-type ament_cmake --dependencies rclcpp robot_interface --node-name nodeName
```
- 使用
```c++
//include such as : 
#include "robot_interface/msg/robot_post.hpp"
#include "robot_interface/msg/robot_status.hpp"

//serviceType such as:
//定义服务类型：
//<robot_interface::srv::MoveRobot>

//server处理函数 param in：
//std::shared_ptr<robot_interface::srv::MoveRobot_Request> request
```
### 2.4 param

### 2.5 action

### 2.6 launch
#### 2.6.1 语法

```python
# 目前最常用的
from launch import LaunchDescription # 导入launch
from launch_ros.actions import Node  # 导入node

# 定义函数，此函数名称为固定的
def generate_launch_description():
    return LaunchDescription([
        Node(
            package = 'TODO:name package',
            executable = 'TODO:name exe name',
            name = 'TODO:name node name'
            parameters = [
                {'param1' : 'description1'},
                {'param2' : 'description2'},
                {'param3' : 'description3'},
                # ...
            ]

        ),
        Node(
            # ...

        ),
    ])
```
#### **2.6.2 dependencies**
- package.xml
  ```xml
  <exec_depend>launch</exec_depend>
  <exec_depend>launch_ros</exec_depend>
  ```
- CMakeLists.txt
  ```cmake
  install(DIRECTORY launch
    DESTINATION share/${PROJECT_NAME})
  ```


## **3 Intermediate Usage**
### **3.1 TF2**
2023/1/21记录，现在ROS2的TF2的教程比较少，官方和网上资源也是比较少，看完ROS2官方教程后可以参考ros1的documents多了解一些，[ROS1TF2Documents链接](http://wiki.ros.org/tf2)  
除此之外，记住这句话：```A frame is a coordinate system```（TF2中将坐标系称为frame，且一般使用**right-handed**）  
tf2简介，tf是transform的简称，2是第二代，tf1已经被弃用。tf2主要有以下几个```namespace``` （ROS1 wiki内）
|name|function|main usage|  
|:---:|:---|---:|
|tf2|tf2原始库|```listener``` & ```broadcaster```|
|tf2_ros|绑定ros和tf2，便于ros使用|详情见下|
|tf2_msgs|tf2通信使用的interface|```TFMessage```|
|tf2_tools|tf2相关工具|```view_frames``` & ```tf2_echo```|
|tf2_eigen|convert tf2 data to eigen data structures|```doTransform(eigen_in,eigen_out,transform)```|
|tf2_geometry_msgs|convert geometry_msgs to tf2 messages||
|tf2_sensor_msgs|convert pointcloud2 messages through a geometry_msgs::Transform||
|tf2_bullet|同上|| 
|tf2_kdl|同上|| 

记录一下几个namespace的主要文件夹，用于学习和后续查阅  
|tf2|tf2_ros|
|:---|:---|
|buffer_core.cpp/h|buffer_interface.h|
|convert.h|buffer_server.cpp/h|
|Quaternion.h|buffer_client.cpp/h|
|Transform.h|buffer.cpp/h|
|cache.xx|message_filter.h|
|Vector3.h|static_transform_broadcaster.cpp/h|
|Matrix3x3.h|transform_broadcaster.cpp/h|
||transform_listener.cpp/h|
|exceptions.h||
|||

#### **3.1.1 安装tf包**
自动识别当前ROS2版本：```$(printenv ROS_DISTRO)``` ---> ```humble``` or ```forxy``` or ```galactic```
```bash
sudo apt-get install ros-$(printenv ROS_DISTRO)-turtle-tf2-py ros-$(printenv ROS_DISTRO)-tf2-tools ros-$(printenv ROS_DISTRO)-tf-transformations
```
#### **3.1.2 TF2工具**
- **view_frames**  
  frames和broadcast图形可视化
  ```bash
  ros2 run tf2_tools view_frames
  ```
- tf2_echo  
  播报两个frame之间的姿态
  格式：```ros2 run tf2_ros tf2_echo [reference_frame] [target_frame]```
- rviz
---
#### **3.1.3 tf2 code**
**常用接口：**
- ```geometry_msgs/msg/TransformStamped```  
  这表示在header.stamp时从坐标帧header.frame_id到坐标帧child_frame_id的转换(当前---->child)  
  ```ros2 interface show geometry_msgs/msg/TransformStamped```
  ```bash
    std_msgs/Header header
        builtin_interfaces/Time stamp
            int32 sec
            uint32 nanosec
        string frame_id

    string child_frame_id

    Transform transform
        Vector3 translation
            float64 x
            float64 y
            float64 z
        Quaternion rotation
            float64 x 0
            float64 y 0
            float64 z 0
            float64 w 1
    ```
- ```tf2_msgs/msg/TFMessage```   
  ```bash
  ros2 interface show tf2_msgs/msg/TFMessage
  # tfs数组
  geometry_msgs/TransformStamped[] transforms
  ```
**```tf2_ros```-namespace:**  
**TransformBroadcaster**    
可以直接看一下源代码，```#include "tf2_ros/transform_broadcaster.h"```,本质上就是一个封装了topic-publisher和node的类，然后调用函数publish ```geometry_msgs::msg::TransformStamped```类型的数据。

- 构造函数  ```TransformBroadcaster(TODO:Node ref)```
- 广播函数   
  ``` void sendTransform(const geometry_msgs::TransformStamped & transform);```
---
**StaticTransformBroadcaster**
- 同TransformBroadcaster

---
**Buffer**   
**源代码文件夹构成**   
首先，说一下```buffer.cpp/h```这个文件夹构成  
buffer部分应该是tf2-Listener最重要的一部分了，其与其他文件的关系如下图所示([截取自ROS1TF2文档](http://docs.ros.org/en/latest/api/tf2_ros/html/c++/buffer_8h.html)):
![img](./noteSrc/CoordinateTransformation/tf2-buffer.xxx.png)
buffer是```tf2_ros```命名空间中的一个重要文件夹，通过阅读ROS1 tf2文档和源代码，发现```buffer.h```所依赖中的重要头文件为```tf2/buffer_core.h和tf2_ros/buffer_interface.h``` 

---

- **```buffer_core.h```**  
  
其中```buffer_core.h```其实才是transform的核心方法所在，里面定义了以下重要函数。列出重要的API      
|函数名|作用|  
|:---|:---|
|```transformMsgToTF2()```|msg--->TFMessage|
|```BufferCore::lookupTransform()```|重载 : 2. 搜索tree结构，并调用```transformMsgToTF2```得到输出```geometry_msgs::tfs```|
|```BufferCore::canTransform()```|重载 : 2, 判断是否存在此```transform```|
---
- **```tf2_ros/buffer_interface.h```**  

![img](./noteSrc/CoordinateTransformation/tf2-tf2_ros-buffer_interface.png)
此文件是虚基类,定义了```tf2_ros```包中的Buffer类的对外接口interface，具有以下接口,函数详细说明见此[链接文档(ROS1TF2wiki)](http://docs.ros.org/en/latest/api/tf2_ros/html/c++/buffer__interface_8h_source.html):<center>
|函数名|作用|  
|:---|:---|
|```lookupTransform()```|两个重载版本|
|``` canTransform()```|两个重载版本|
|```transform()```|多个函数重载|
</center>

---

- **```tf2_ros/buffer.h```**
  
此文件夹中只有```Buffer```这个class，其实例化了```BufferInterface```和```AsyncBufferInterface```，继承了```BufferCore```，所以此类的内容很简单，基本就是调用```BufferCore```类中实现的方法,所以其外接函数主要就是以下两个函数<center>
|函数名|作用|说明|继承或实现自|
|:---|:---|:---|:---|
|```lookupTransform()```|查找tf|实现，四个重载版本|```BufferInterface```|
|``` canTransform()```|isTF？|实现，四个重载版本|```BufferInterface```|
|```waitForTransform()```|异步等待|实现，两个重载版本|```AsyncBufferInterface```|
|```transform()```|in转换为out|继承|```BufferInterface```|
</center>

<br>

---
**TransformListener**  
本质就是一个topic-subscriber结点，具有以下重要的数据成员,可以看到其实就是一个结点对象，之后就是两个```TFMessage```的subscription，分别对应dynamic和static,在之后就是最重要的```tf2::BufferCore```了，他是比较重要的，担任了坐标转换的绝大部分工作，详情请见上方Buffer部分的笔记
- **membership**
```cpp
rclcpp::Node::SharedPtr optional_default_node_ = null
ptr;
rclcpp::Subscription<tf2_msgs::msg::TFMessage>::SharedPtr message_subscription_tf_;
rclcpp::Subscription<tf2_msgs::msg::TFMessage>::SharedPtr message_subscription_tf_static_;
tf2::BufferCore & buffer_;
```
- **public interface**  

TransformListener类的public interface只有一个，那就是类的构造函数，参数为```tf2::BufferCore &buffer```，但我们一般应用时传入的是```tf2_ros::Buffer```类型的数据，通过构造一个这样的TransformListener，构造函数时就会将订阅器设置好，应该是将Listerner和buffer绑定在一起了，listener负责设置buffer，buffer负责获取和存储tf数据，然后获取transform信息，需要通过```tf2_ros::Buffer buffer```类的```lookTransform()```方法进行获取  

--- 

<br>

**MessageFilter**  
---
**作用:**  
**The ```tf2_ros::MessageFilter``` will take a subscription to any ROS 2 message with a header and cache it until it is possible to transform it into the target frame.**  

```MessageFilter```此类继承自```message_filters::SimpleFilter<MSGType>```,并实现了```MessageFilterBase```虚基类    
<br>

其中虚基类中有四个外部public接口:
```cpp
//清空queue列表中的所有message
virtual void clear() = 0;
//设置目标frame
virtual void setTargetFrame(const std::string & target_frame) = 0;
virtual void setTargetFrames(const V_string & target_frames) = 0;
//设置超时忍受时间
virtual void setTolerance(const rclcpp::Duration & tolerance) = 0;
```
<br>

而其还继承了```message_filters::SimpleFilter<MSGType>```这个类，所以也需要简单了解这个类构成，除此之外还了解了一下```message_filters```这个namespace的功能和构成.参考文档：[ROS1wiki](http://wiki.ros.org/message_filters)  

**```message_filters```**  

**设计模式Filter Pattern**  
- Inputs are connected either through the filter's ```constructor``` or through the ```connectInput()``` method.
  ```c++
  A_filter A; 
  // contact A and B --- constructor
  B_filter B(A);
  // contact A and C --- connectInput
  C_filter C;
  C.connectInput(A);
  ```   
- Outputs are connected through the ```registerCallback()``` method.
  ```cpp
  // 连接filter的输出和自己的callback函数
  B.registerCallback(myCallbackFun);

  //此函数会返回message_filters::Connection 类型的对象,可以通过调用disconnect()断开链接
  ```
---
**```message_filters::Subscriber```**  
&emsp; The Subscriber filter is simply a wrapper around a ROS subscription that provides a source for other filters. The Subscriber filter cannot connect to another filter's output, instead it uses a ROS topic as its input. 

&emsp; ```message_filters::Subscriber``` filter仅仅是ROS subscription的一个wrapper(封装)类，为其他filters提供source。因此其不能像pattern中一样进行连接，取而代之的需要使用ROS 的```topic```作为他的输入。  
&emsp; 自我感觉这个想是一个为了和ros交互方便的一个设计.示例代码如下所示:(<font color=Red>**这里做了ROS版本迁移可能有误，仅供参考,ROS1(官方文档)-->ROS2(下述代码)，不知正确与否**</font>)
```cpp
message_filters::Subscriber<geometry_msgs::msg::PointStamped> sub(this, "my_topic"); //param: node_ptr, "topic name" 
 //订阅Callback
sub.registerCallback(&PoseDrawer::msgCallback,this);
```
is the equivalent of:
```cpp
rclcpp::Subscription<geometry_msgs::msg::PointStamped>::SharedPtr sub =this->create_subscription<geometry_msgs::msg::PointStamped>("my_topic",1,myCallback);
```
```message_filters::Subscriber``` class 定义在```#include "message_filters/subscriber.h"```文件中,该文件的dependencies如下:  
![img](./noteSrc/CoordinateTransformation/tf2-message_filters-subscriber.png)

|```message_filters``` ns相关文件夹|main class|作用|
|:---|:---|:---|
|```mf/connection.h```|```Connection```|registerCallback()回调函数返回```Connection```类型对象|
|```mf/simple_filter.h```| ```SimpleFilter``` |使用```SimpleFilter```类|
|```mf/message_event.h```|```MessageEvent```|通过message创建```MessageEvent```对象|
|```mf/parameter_adapter.h```|```ParamAdapter```|```ParamAdapter```类封装```MessageEvent```的struct,仅有一个方法```getParameter()```，返回constMSGptr类型数据|
|```mf/signal1.h```|```Signal1```|```callback```操作-```add、remove 和 call```|

(以上```mf``` 为 ```message_filters```缩写)

<br>

该类inherit(继承)了```message_filters::SubscriberBase```和```message_filters::SimpleFilter```  
&emsp;其中```SubscriberBase```为```Subscriber```的虚基类.```virtual```方法主要是```subscribe()```,其有5个重载版本，比较常用的两个如下所示:
```cpp
subscribe(nodePtr, topicName, qos);
subscribe(nodePtr, topicName, qos, SubscriptionOptions);
```
&emsp;而SimpleFilter也最多应用```registerCallback```绑定callback函数。因此总上Subscriber应用时也仅仅是创建一个sub。





---  
以下内容暂时没用到，等待后续更新  
```message_filters::TimeSynchronizer```  
```message_filters::TimeSequencer```
```message_filters::Cache```
```message_filters::sync_policies::ExactTime```
```message_filters::sync_policies::ApproximateTime```
```Chain```



<br>

---
**```tf2_ros::MessageFilter```函数接口API**
- **```MessageFilter```**(构造函数)
  ```cpp
  //重载版本1
  MessageFilter(
  BufferT & buffer,   
  const std::string & target_frame,
  uint32_t 
  queue_size, //0表示infinite，无限，所以一定要设置一个大于0的数
  const rclcpp::node_interfaces::NodeLoggingInterface::SharedPtr & node_logging,
  const rclcpp::node_interfaces::NodeClockInterface::SharedPtr & node_clock,

  std::chrono::duration<TimeRepT, TimeT> buffer_timeout =
  std::chrono::duration<TimeRepT, TimeT>::max())

  //重载版本2
  MessageFilter(
  F & f,  //The filter to connect this filter's input to. Often will be a message_filters::Subscriber.
  ...
  //剩下六个参数同上
  )
  ```
  构造函数中调用了一下三个函数
  ```cpp
  init(); //初始化类成员变量
  setTargetFrame(target_frame); //setTargetFrame
  connectInput(f);//见下,下述理解可能有误，仅供参考，详见源码
  ```
---
- **```connectInput()```**  
  Connect this filter's input to another filter's output. If this filter is already connected, disconnects first.
  函数参数一般是class : ```message_filters::Subscriber```
  ```cpp
  // 源码比较简单，可以直接看一下
  void connectInput(F & f)
  {
    message_connection_.disconnect();
    message_connection_ = f.registerCallback(&MessageFilter::incomingMessage, this);
  }
  ```
    首先断开已有链接，然后进行新的链接。而后就是封装```message_filters::SimpleFilter<MSGType>```中的方法,就是将输入过来的message和滤波器MessageFilter此类绑定起来，使用的函数是```registerCallback(MessageFilter::incomingMessage,this)```，他将callback函数(param1)与此类绑定在一起。内部是调用的```MessageFilter::add()```函数，将message添加到队列
---


- **```setTargetFrame()```&```setTargetFrames()```**  
  功能如其名称,  
  **param**  : ```const std::string & target_frame ```   
  **retval** : ```void```
---
- **```setTolerance()```**  
  设置忍受限度，也就是超时时间timeout  
  **param**  : ```const rclcpp::Duration & tolerance```  
  **retval** : ```void```
---
- **```clear()```**  
  清空queue中的message
---
- **```setQueueSize()```**   
  功能如其名称
---
- **```registerCallback()```**  
  函数原型
  ```cpp
  template<typename T, typename P>
  Connection registerCallback(void(T::*callback)(P), T* t)
  ``` 
  用在```MessageFilter```中参数含义如下   
  **param1**  : ```&UserClassName::msgCallback``` 回调函数  
  **param2** : ```this```, UserClassName  
  另外，要知道的是此函数不是```MessageFilter```类定义的，而是从```message_filters::SimpleFilter<Type>```类中public继承过来的。所以想搞懂```MessageFilter```的基本原理，也需要简单了解下```message_filters```这个ros基本类库的基本构成和使用方法,参考文档：[ROS1wiki](http://wiki.ros.org/message_filters)



**Exceptions**  
定义的这个exceptions都在```#include "tf2/exceptions.h"```这个文件夹中，tf2中定义了以下几个Exceptions.<center>
|exception|功能|备注|
|:---|:---|:---|
|```TransformException```|A base class for all tf2 exceptions|public继承自```std::runtime_error```|
|```ConnectivityException```|两个frame在Reference Frame tree中未连接|public继承自```TransformException```|
|```LookupException```|frame_id出错,tarFrame未在Reference Frame tree中|public继承自```TransformException```|
|```ExtrapolationException```|访问超出当前限制的数据|public继承自```TransformException```|
|```InvalidArgumentException```|参数错误，例如:Quaternion (0,0,0,0)|public继承自```TransformException```|
|```TimeoutException```|超时|public继承自```TransformException```|
</center>


<br>
<br>

**Application**


**静态转换广播器 StaticTranformBroadcaster**  

这个ros已经封装好了包，我们只需要调用即可。  
cmd 格式:
```bash
ros2 run tf2_ros static_transform_publisher --x x --y y --z z --qx qx --qy qy --qz qz --qw qw --frame-id frame_id --child-frame-id child_frame_id
```
此node本质上是一个```topic-publisher```.    
**topic name**为：```/tf_static```  ,它会publish 从```frame-id```到```child-frame-id```的平移向量-translation和旋转四元数-rotation，节点运行的信息如下所示：
```
[INFO] [1674102155.995393457] [static_transform_publisher_GwthsV8HNvh5sHcC]: Spinning until stopped - publishing transform
translation: ('1.000000', '2.000000', '3.000000')
rotation: ('0.000000', '0.000000', '0.000000', '1.000000')
from 'world' to 'turtle1'
```
其中topic的消息类型msg为：```tf2_msgs/msg/TFMessage```
```bash
ros2 interface show tf2_msgs/msg/TFMessage
# 则会显示其msg的具体构成，如下所示，其实就是一个数组geometry_msgs/TransformStamped[] transforms
```
写成launch格式的话如下所示：
```python
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    return LaunchDescription([
        Node(
            package = 'tf2_ros',
            executable = 'TODO:rename by yourself.origin name :static_transform_publisher',
            arguments = ['--x', '0', '--y', '0', '--z', '1', '--yaw', '0', '--pitch', '0', '--roll', '0', '--frame-id', 'TODO:rename parent', '--child-frame-id', 'TODO:rename child']
        )
    ])
```

<br>

---
**转换广播器 TranformBroadcaster**   
- **include**  
```cpp
#include "tf2_ros/transform_broadcaster.h"
#include "geometry_msgs/msg/transform_stamped.hpp"
#include "tf2/LinearMath/Quaternion.h"
```
- **frame_id & param declare**  
  frame_id 通过名称唯一确定，因此可以使用param功能，创建结点时传入参数。
  ```cpp
  private:
    std::string mFrameID;
    std::string mChildFrameID;

  //constructor 中declare:
  mFrameID = this->declare_parameter<std::string>("frame_id","name1");
  mChildFrameID = this->declare_parameter<std::string>("child_frame_id","name2");
  ```
- **broadcaster create**
  ```cpp
  //statement:
  std::shared_ptr<tf2_ros::TransformBroadcaster> mTFBroadcaster;
  //create:
  mTFBroadcaster = std::make_shared<tf2_ros::TransformBroadcaster>(*this);//param : node
  ``` 
- **tfs**
  ```cpp
  //define variable
  geometry_msgs::msg::TransformStamped tfs;
  //or
  //std::vector<geometry_msgs::msg::TransformStamped> tfs_vec;

  //set header
  tfs.header.stamp = this->get_clock()->now();
  tfs.header.frame_id = mFrameID;
  
  //set child_frame_id
  tfs.child_frame_id = mChildFrameID;

  //set transform-translation
  tfs.transform.translation.x = ...;
  tfs.transform.translation.y = ...;
  tfs.transform.translation.z = ...;

  //set transform-rotation
  tf2::Quaternion quat;
  //TODO:Assign a value to quat
  tfs.transform.rotation.x = quat.x();
  tfs.transform.rotation.y = quat.y();
  tfs.transform.rotation.z = quat.z();
  tfs.transform.rotation.w = quat.w();
  ```
- **send**  
  ```
  mTFBroadcaster->sendTransform(ts);
  ```
- launch
  ```py
  from launch import LaunchDescription
  from launch_ros.actions import Node

  def generate_launch_description():
      return LaunchDescription([
          Node(
              package = 'myPkgName',
              executable = 'myExeName',
              name = 'nodeName',
              parameters = [
                  {'mFrameID': 'name1'},
                  {'mChildFrameID': 'name2'}
              ]
          ),
      ])
  ```
<br>

---
**转换监听器 TranformListener** 
- **```include```**
  ```cpp
  #include "geometry_msgs/msg/transform_stamped.hpp"

  //tf2_ros
  #include "tf2_ros/transform_listener.h" 
  #include "tf2_ros/buffer.h"
  #include "tf2_ros/create_timer_ros.h"

  //tf2
  #include "tf2/exceptions.h"
  ```
- **```target_frame & self_frame & param```**  
  依旧如上，需要声明param,并传参，来确定转换的dst frame 和 src frame

- **```listener & buffer```**
  ```cpp
  /*** statement ***/
  std::shared_ptr<tf2_ros::TransformListener> mTFListener;

  std::unique_ptr<tf2_ros::Buffer> mTFBuffer  ;
  
  /*** init ***/
  mTFBuffer = std::make_unique<tf2_ros::Buffer>(this->get_clock());
  mTFListener = std::make_shared<tf2_ros::TransformListener> (*mTFBuffer);
  ```
- **```lookTransform```**
  ```cpp
  geometry_msgs::msg::TransformStamped tfs = mTFBuffer->lookupTransform(
      "toFrameRel", "fromFrameRel",
      tf2::TimePointZero);
  /** 
   * param1： dst frame，string 按需制定
   * param2： src frame，string 按需制定
   * param3： time,上述tf2::TimePointZero 表示使用latest message
   */
  ```
  PS:一般和try catch一起使用
  ```cpp
  try{
    lookTransform();
  } catch (const tf2::TransformException & ex){
    RCLCPP(this->get_logger(),"%s",ex.what());
    return;
  }
  ```
- **使用tfs数据**
  ```
  tfs.transform.rotation.xxx
  tfs.transform.translation.xxx
  ```
  

<br>

---
**消息滤波器 MessageFilter** 
- **```include```**
  ```cpp
  //ps
  #include "geometry_msgs/msg/point_stamped.hpp" //get point msg
  //mfs
  #include "message_filters/subscriber.h" //F
  //tf2_ros
  #include "tf2_ros/buffer.h"
  #include "tf2_ros/create_timer_ros.h"
  #include "tf2_ros/message_filter.h"
  #include "tf2_ros/transform_listener.h"
  //tf2
  #include "tf2_geometry_msgs/tf2_geometry_msgs.hpp"
  ```
- **```statement variables```**
  - tar_frame
  - tf2_buffer
  - listener
  - mfs_sub
  - tf2_filter
  ```cpp
  std::string m_tar_frame_;
  std::shared_ptr<tf2_ros::Buffer> m_tf2_buffer_;
  std::shared_ptr<tf2_ros::TransformListener> m_tf2_listener_;
  message_filters::Subscriber<geometry_msgs::msg::PointStamped> m_point_sub_;
  std::shared_ptr<tf2_ros::MessageFilter<geometry_msgs::PointStamped>> m_tf2_filter_;
  ```

- **```target_frame```**
  ```cpp
  m_tar_frame_  = this->declare_parameter<std::string>("target_frame","myTarName");
  ```
- **```init buffer```**
  ```cpp
  m_tf2_buffer = std::make_shared<tf2_ros::Buffer>(this->get_clock());
  
  //Create the timer interface before call to waitFoTransform to avoid tf2_ros::CreateTimerInterfaceException exception
  auto timer_interface = std::make_shared<tf2_ros::CreateTimerROS>(
    this->get_node_base_interface(),
    this->get_node_timers_interface();
    );
  m_tf2_buffer_.setCreateTimerInterface(timer_interface);
  ```
- **```init listenr```**
  ```cpp
  m_tf2_listener_ = std::make_shared<tf2_ros::TransformListener>(*m_tf2_buffer_);
  ```
- **```mfs_sub.subcribe()```**
  ```cpp
  m_point_sub_.subscribe(this,"topic name");
  ```
- **```init tf2_filter```**
  ```cpp
  //define timeout
  std::chrono::duration<int> buffer_timeout(1);
  m_tf2_filter_ = std::make_shared<tf2_ros::MessageFilter<geometry_msgs::msg::PointStamped>>(
    m_point_sub_,   //F,subscriber
    *m_tf2_buffer,  //buffer
    m_tar_frame_,   //tar frame
    100,            //queue_size
    this->get_node_logger_interface(),  //logger
    this->get_node_clock_interface(),//clock
    buffer_timeout//buffer timeout
  );
  ```
- **```callback绑定```**
  ```cpp
  m_tf2_filter_->registerCallback(&MyClassName::myMsgCallBackName,this);

  private:
  void myMsgCallBackName(const geometry_msgs::msg::PointStamped::SharedPtr point_ptr){
    geometry_msgs::msg::PointStamped point_out;
    try {
        tf2_buffer_->transform(*point_ptr, point_out, m_target_frame_);
        RCLCPP_INFO(
                this->get_logger(), "Point of turtle3 in frame of turtle1: x:%f y:%f z:%f\n",
                point_out.point.x,
                point_out.point.y,
                point_out.point.z);
    } catch (const tf2::TransformException & ex) {
        //TODO: RCLCPP_WARN()...
    }
  }
  
  ```

<br>

---
**异常处理 Exceptions**   
一般来看是这样使用的
```cpp
try{
  //...
}
catch(const tf2::XxxxExpection ex){
  RCLCPP_INFO(this->get_logger(), "%s",ex.what();
}
```







<br>
<br>

## 4 建模与仿真
### 4.1 建模
#### 4.1.1 URDF
URDF（Unified Robot Description Format）统一机器人描述格式
```xml
<!--主标签-->
<robot name = "robotname"><!--此间描述机器人构成--></robot>
<link name = "linkname" ><!--此间描述各个小组件实体--> </link>

<joint name = "jointname" type = "这里有很多选择，见下"><!--此间描述组件构成的关节--></joint>
<!--通过parent和child将两者连接起来形成joint-->
<parent link = "parent_link_name"/>
<child link = "child_link_name" />
<!--
type :      关节类型，可选如下
revolute:   旋转关节，绕单轴旋转,角度有上下限,比如舵机0-180
continuous: 旋转关节，可以绕单轴无限旋转,比如自行车的前后轮
fixed:      固定关节，不允许运动的特殊关节
prismatic:  滑动关节，沿某一轴线移动的关节，有位置极限
planer:     平面关节，允许在xyz，rx ry rz六个方向运动
floating:   浮动关节，允许进行平移、旋转运动
-->

<!--子标签-->
<visual><!--visual子标签就可以声明出来机器人的visual形状--> </visual>
<!--几何形状，such as:-->
<geometry> <cylinder length="0.12" radius="0.10"/> </geometry>
<geometry> <sphere radius="0.016"/> </geometry>
<mesh filename="package://robot_description/meshes/base_link.DAE"/><!--第三方导入的--> 
<!--初始位置-->
<origin xyz="0 0 0" rpy="0 0 0" />
<!--材质-->
<material name="white"><color rgba="1.0 1.0 1.0 0.5" /> </material>

<!--
link标签下添加collison子标签,其中collision可以包含的子标签如下：
origin:碰撞体的中心位姿
geometry:碰撞检测的几何形状
material：材料
-->
<collision>
    <origin xyz="0 0 0.0" rpy="0 0 0"/>
    
    <geometry>
        <cylinder length="0.12" radius="0.10"/>
    </geometry>
    
    <material name="blue">
        <color rgba="0.1 0.1 1.0 0.5" />
    </material>
</collision>

<!--转动惯量，子标签，mass和inertia-->
<inertial>
  <mass value="0.2"/>
  <inertia ixx="0.0122666" ixy="0" ixz="0" iyy="0.0122666" iyz="0" izz="0.02"/>
</inertial>
```
#### 4.1.2 可视化URDF-RVIZ
```cpp 
TODO：这里太专一，后续总结好在补充
```
- 安装库指令
```shell
sudo apt install ros-$ROS_DISTRO-joint-state-publisher-gui 
sudo apt install ros-$ROS_DISTRO-robot-state-publisher
```
- URDF编写文件
- python编写launch  

URDF-RVIZ可视化的node和topic rqt图形
![img](./noteSrc/rqt-graph-rvizCar.png)

#### 4.1.3 Gazebo模型加载
- 安装gazebo-ros2插件
```shell
sudo apt install ros-humble-gazebo-ros
```
- 启动gazebo和其插件
```shell
gazebo --verbose -s libgazebo_ros_init.so -s libgazebo_ros_factory.so 
```
- 调用service加载urdf模型
[具体流程网址](https://fishros.com/d2lros2/#/humble/chapt9/get_started/3.%E5%9C%A8Gazebo%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%99%A8%E4%BA%BA%E6%A8%A1%E5%9E%8B)  
查看gazebo中的服务接口类型
```
ros2 node list  
    /gazebo

ros2 service list 
    /delete_entity  # 删除实体模型
    /get_model_list # 得到gazebo现有的模型列表
    /spawn_entity   # 删除gazebo中已经加载好的模型

ros2 ros2 service type /spawn_entity # 查看加载模型这个服务的service type
    gazebo_msgs/srv/SpawnEntity

ros2 interface show gazebo_msgs/srv/SpawnEntity
    string name                       # 加载的实体名字Name of the entity to be spawned (optional).
    string xml                        # 实体描述，string or URDF or SDF.
    string robot_namespace            # Spawn robot and all ROS interfaces under this namespace
    geometry_msgs/Pose initial_pose   # Initial entity pose.
    string reference_frame            # initial_pose is defined relative to the frame of this entity.
                                      # If left empty or "world" or "map", then gazebo world frame is
                                      # used.
                                      # If non-existent entity is specified, an error is returned
                                      # and the entity is not spawned.
    ---
    bool success                      # Return true if spawned successfully.
    string status_message             # Comments if available.
```
然后可以使用rqt中的service caller调用服务加载模型

### 4.2 gazebo仿真
#### 4.2.1 仿真插件学习使用
- 输入参数
  - 一系列的参数指定
- 两轮差速控制器
  - topic : cmd_val 获取目标线速度和角速度
  - topic type：geometry_msgs/msg/Twist # 包含了三轴线速度和角速度
- 输出参数
  - topic : odom
  - topic type : nav_msgs/msg/Odometry





**如果有以下问题**
```
[gazebo-1] [Err] [Plugin.hh:212] Failed to load plugin libgazebo_ros_ray_sensor.so: 
libgazebo_ros_ray_sensor.so: cannot open shared object file: No such file or directory

[gazebo-1] [Err] [Plugin.hh:212] Failed to load plugin libgazebo_ros_diff_drive.so: 
libgazebo_ros_diff_drive.so: cannot open shared object file: No such file or directory
```
ls查看一下ros中lib的相关gazebo是否存在
```
ls /opt/ros/humble/lib/libgazebo_ros*
```
如果不存在，这说明是有一些gazebo的.so库未安装全，执行一下以下指令安装
```
sudo apt install ros-$ROS_DISTRO-gazebo-ros-pkgs
```
多插件截图，gazebo_
- odom
- imu
- laserscan
- fishbot  
都是topic，可以通信
![img](./noteSrc/gazebo-plugin.png)
#### 4.2.2 ladar plugin
激光雷达仿真可视化，先记录一下，仿真的实现流程
........
![img](./noteSrc/gazebo-LaserRviz.png)


## 5 Navigation
### 5.1 SLAM
#### 5.1.1 Cartographer
##### 5.1.1.1 安装
编译安装
```shell
# 安装cartographer
sudo apt install ros-humble-cartographer
# 安装cartographer-ros
sudo apt install ros-humble-cartographer-ros
# 判断是否安装成功
ros2 pkg list | grep cartographer 
## 出现以下结果
# cartographer_ros
# cartographer_ros_msgs
```
源码安装(暂时不用)
- git clone
```shell
git clone https://ghproxy.com/https://github.com/ros2/cartographer.git -b ros2
git clone https://ghproxy.com/https://github.com/ros2/cartographer_ros.git -b ros2
```
##### 5.1.1.2 
lua配置文件的根路径
```
opt/ros/humble/share/cartographer/configuration_files
/opt/ros/humble/share/cartographer_ros/configuration_files
```
文件
```
# 建图
map_builder.lua         
map_builder_server.lua  

# 后端优化
pose_graph.lua           

# 前端  
trajectory_builder_3d.lua
trajectory_builder_2d.lua  
trajectory_builder.lua
```

# 相机模型
## 相机坐标系
![img](./noteSrc/camera_coordinate_system.png)

## mindvision配置过程
1 官网下载linux-SDK，[下载链接](https://www.mindvision.com.cn/rjxz/list_12.aspx?lcid=138)  
2 解压进入文件夹，打开readme.pdf读完  
3 运行install.sh
```bash
sudo su
./install.sh
# 然后重启
```
4 配置相机权限  
首先查看是否识别到相机
```bash
lsusb

# 出现这个则表示识别到USB相机
# f622：idVendor d132:idProduct
Bus 004 Device 005: ID f622:d132 MindVision SUA133GC
``` 
把88-mvusb.rules和99-mvusb.rules复制到 ```/etc/udev/rules.d```文件夹中，然后```sudo gedit 88-mvusb.rules```.
```
KERNEL=="*", ATTRS{idVendor}=="f622", ATTRS{idProduct}=="d132", MODE:="0777", SYMLINK+="mindvision" 
```
运行```demo/python_demo/```路径下的grub.py
```
python3 grab.py
```
抓取到图片的话说明相机可以用
  
自己工程中将linuxsdk中的include文件夹和lib文件夹中对应自己的平台对应的文件夹加入自己的工程，相当于src



# 串口通信
## 查看设备名称  
```bash
ll /dev
# 出现以下类似的就是成功识别
crw-rw---- 1 root dialout 188, 0 Aug  3 21:46 /dev/ttyUSB0
```
## 给予串口权限
```bash
sudo chmod 777 /dev/ttyUSB0 #按照自己的设备名字对ttyUSB0进行更改
```
## 安装ch340驱动
[ubuntu20.04-安装教程链接](https://blog.csdn.net/qq_27558597/article/details/117900705)
但是Ubuntu22.04安装CH340的驱动时出现了一些问题，暂时搁置，下赛季更新

# 坐标转换
主要是相机坐标系和世界坐标系的转换，两者之间可以通过刚体运动(旋转和平移)进行转换，其中世界坐标系一般选用云台的IMU模块作为云台姿态获取方式,IMU可以选择C板或者外接IMU(WIT901C-TTL)
## **坐标系**
### 普通三维坐标系
### **齐次坐标系**
旋转矩阵和平移向量整合到一起的矩阵，称为齐次坐标系矩阵: <center> $Transform  = \left[\begin{matrix}
    R_{3*3} & T_{3*1}\\
    0_{3*3} & 1 
\end{matrix}\right]$ </center>
其中$R$为旋转矩阵

## **旋转与平移**
### 旋转  
我们一般使用右手坐标系，如下图所示：

### **1.1 欧拉角**
| 角     | 旋转轴 | 方向                        |    
|:------|:---:|:--------------------------|  
| yaw   |  z  | 顺着z轴负方向看，xoy逆时针转动为正,顺时针为负 |  
| pitch |  y  | 顺着y轴负方向看，xoz逆时针转动为正,顺时针为负 |
| roll  |  x  | 顺着x轴负方向看，yoz逆时针转动为正,顺时针为负 |  

![img](./noteSrc/CoordinateTransformation/rotation-eular.png)

### **1.2 旋转矩阵**
旋转矩阵，是旋转的另一种表示方式，表示同一个点在两种坐标系之间的转换约束关系，绕三轴旋转分别对应三个旋转矩阵。
- 绕x轴旋转 $\alpha$ 
$$
R(x,\alpha) = 
 \left[
 \begin{matrix}
   1 & 0 & 0 \\
   0 & \cos\alpha & -\sin\alpha \\
   0 & \sin\alpha & \cos\alpha
  \end{matrix}
  \right] \tag{roll}
$$

- 绕y轴旋转 $\beta$ 
$$
R(y,\beta) = 
 \left[
 \begin{matrix}
   \cos\beta & 0 & \sin\beta \\
   0 & 1 & 0 \\
   -\sin\beta & 0 & \cos\beta
  \end{matrix}
  \right] \tag{pitch}
$$

- 绕z轴旋转 $\gamma$ 
$$
R(z,\gamma) = 
 \left[
 \begin{matrix}
   \cos\gamma & -\sin\gamma & 0 \\
   \sin\gamma & \cos\gamma & 0 \\
   0 & 0 & 1
  \end{matrix}
  \right] \tag{yaw}
$$

这里旋转矩阵的关系一定要搞清楚，他是将变换后的矩阵转变会原来的坐标系，这里就以绕Z轴旋转举个例子，原始坐标系成为A-坐标系，将其绕Z旋转$\theta$度(顺着Z轴负方向,逆时针旋转$\theta$)，然后得到了B-坐标系。旋转矩阵为R(z,$\theta$),那么两轴之间的坐标转换关系为: <center>$\left[\begin{matrix}
    x_A  \\ y_A \\ z_A
    \end{matrix}\right]$  = $R$(z,$\theta$) * 
    $\left[\begin{matrix}
    x_B  \\ y_B \\ z_B
    \end{matrix}\right]$</center>

坐标系旋转有顺序，各轴具体的旋转矩阵根据旋转角度按照上面的矩阵进行计算得到，除了角度之外，三轴的旋转顺序也有很重要的影响，相同的角度而不同的旋转顺序也会导致不一样的旋转矩阵.例如这里使用ZYX的旋转顺序，将坐标系A转变到B.则
旋转矩阵为:<center>$P_{A_{3*1}} = R(x,\alpha)*R(y,\beta)*R(z,\gamma) * P_{B_{3*1}}$ </center>


### **1.3 四元数**
阿巴啊巴(此处省略N个字)，注意顺序 ：

$q = w +x∗\vec{i}+y∗\vec{j}+z∗\vec{k}$

PS-集中表现方式的转换库-python ```transforms3d```
```bash
# 安装
pip install transforms3d -i https://pypi.tuna.tsinghua.edu.cn/simple
```
 ## 2 23RMSentry








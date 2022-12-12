# ROS2安装方案-小鱼一键安装
```
wget http://fishros.com/install -O fishros && . fishros
```
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
## 2 


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
---
## 4 topic
topic无返回，适合于单向或者大量数据传递
### ros2 topic 
使用的demo
```
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
<depend>rosidl_default_generators</depend>
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

## 3 Adcanced Usage
### 3.1 tf2


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







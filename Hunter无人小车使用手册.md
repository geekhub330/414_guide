## 无人小车构成 

- 小车主要由阿克曼转向机器人底盘、工控机、激光雷达、IMU、以及单目相机构成同时挂载有一个便携显示器如下所示

![hunter1.jpg](/pics/hunter_guide/hunter1.jpg)

- 其中车身两侧的红色按钮为紧急停止按钮，一旦按下会同时切断车身的动力电源与工控机电源，因此非车辆失控情况避免触碰，如无意按下需进行旋转归位

## 传感器参数

- 无人车底盘：松灵机器人Hunter se，详见传感器手册
- 激光雷达：速腾16线激光雷达，详见传感器手册
- 工控机：NUC极摩客-M2，详见官网
- IMU：超核电子CH104M，可切换六轴与九轴模式，默认使用六轴，详见传感器手册
- 相机：单目工业相机与双目深度RGB相机OAK-D-PRO-W

工业相机为100度视场角，无畸变，3.6mm焦距，全局快门曝光，黑白成像，100万像素，详细参数如下,OAK相机请自行查阅官方，会有详尽的参数

![camera_info.png](/pics/hunter_guide/camera_info.png)

## 小车操作（更详细的步骤请参阅hunterse用户手册）

### 遥控器

- 小车具有无线遥控器用于切换手动操作模式与指令控制模式，同时按住屏幕两侧按键进行开机，关机同理
- 上方拨杆SWB为控制模式选择，拨杆只有在中间时为手动控制模式，其它两种均为指令控制，SWD控制灯光的开启和关闭，！！！注意：灯光开启时车辆的最高速度会慢于灯光关闭时，其余拨杆均未定义
- 遥控器使用四节可充电式锂电池，需进行不定时充电

### 底盘

- Q1为充电口，低于电量20%及时充电，充电时长大约在5-6小时，禁止隔夜充电
- 小车电源开启步骤分为两步，第一步先转动车后方旋钮进行底盘电源开启，如下图转动Q2

![hunter2.png](/pics/hunter_guide/hunter2.png)

- 第二步开启工控机右侧的绿色按钮如图

![hunter3](/pics/hunter_guide/hunter3.png)

- 显示器会亮起，随后进入到一个启动选择界面，工控机一共安装了两个版本的Ubuntu系统，分别为Ubuntu20.04(ROS1)和Ubuntu22.04(ROS2)，如果看到选择界面有出现20.04的选项，则默认的第一个选项即为22.04，反之同理，利用所配的手柄式便携遥控器选择合适的版本进入相应系统
- **关机时同样注意先利用键盘关闭Ubuntu系统，即关闭工控机，然后再关闭底盘电源，即旋转旋钮Q2关闭**

### 系统操作
**要使用ROS通讯控制，首先要在主目录下运行脚本文件打开can口通讯**

- 操作如下：Ubuntu22.04下快捷键打开终端，运行./bringup_can2usb_500k.bash
- Ubuntu20.04下同理./bringup_can2usb.bash
**打开can口通信后可以启动ROS的控制**
- 进入到Ubuntu系统后,进入主目录，找到两个文件夹，一个名为hunter_ws,另一个为hunter_nav2_ws
- 第一个工作空间hunter_ws包含有小车所有的传感器驱动文件，包括激光雷达、IMU以及车辆底盘与ROS的通讯控制
- 第二个工作空间hunter_nav2_ws为ROS2专属的一键导航模块
- ctrl+alt+t打开终端或者进入hunter_ws目录内使用鼠标右键打开终端，两种效果相同
- 如果直接使用快捷键打开终端需要cd ~/hunter_ws，这样就进入到该工作空间下



**现在可以开启小车底盘的一键启动程序**

- ROS1：**ROS1下只包含车辆底盘与ROS的通讯没有集成传感器驱动，需自行单独启动**

``` 
确保终端在hunter_ws目录下
source devel/setup.bash
roslaunch hunter_bringup hunter_robot_base.launch
```
- ROS2：**ROS2包含了一键启动激光雷达以及IMU同时融合了轮式编码器与IMU的输出**
```
同理确保终端在hunter_ws目录下
source install/setup.bash
ros2 launch hunter_bringup hunter_bringup.launch.py
```
**此时使用ros的话题命令可以查看当前已启动的话题名称**
- ROS1：
```
rostopic list
```
- ROS2：
```
ros2 topic list
```

### 开启Nav2导航模块
- ROS2集成了一个开箱即用的导航模块Navigation2，已在Ubuntu22.04中集成
- 找到主目录下的hunter_nav2_ws文件夹即为该模块启动程序
```
打开终端cd ~/hunter_nav2_ws
source install/setup.bash
ros2 launch hunter_nav2_bringup bringup.launch.py
```
- 至此可以开启一键导航模式，但目前为止导航参数仍需调整，详见目录下yaml文件或访问nav2官方中文网
### 远程连接
- 小车已安装有远程局域网控制软件rustdesk
**https://github.com/rustdesk/rustdesk/releases/tag/1.2.3-2** 访问官网下载对应的电脑控制端
- 确保在同一局域网下即可通过电脑端ip直连
- 通常查看小车断ip地址的方法为打开终端输入
```
ifconfig
```
- 192.168.xx.xx的即为小车端ip
**不推荐使用配对码链接，因该软件为国外开源项目服务器架设未在国内会有较大延迟**
**如需使用远程配对码连接也可考虑todesk等国内远程控制软件自行安装**
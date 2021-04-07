# # Easy IoT实现mqtt实验



## 一、实验目的及要求

【实验一】实现Easy IoT配置。

【实验二】实现Easy IoT上mqtt消息的通讯。



## 二、实验原理与内容

实现mind+下Easy IoT上mqtt消息的通讯。



## 三、实验软硬件环境

硬件：掌控板

软件：Mind+



## 四、实验过程

1、在https://iot.dfrobot.com.cn/上注册账号并登录，然后点击添加两个新设备

![image-20210407112828540](C:\Users\梁庭锋\AppData\Roaming\Typora\typora-user-images\image-20210407112828540.png)

在Mind+上新建项目，建立程序，其中MQTT初始化参数为：

![image-20210407113040817](C:\Users\梁庭锋\AppData\Roaming\Typora\typora-user-images\image-20210407113040817.png)

参数在https://iot.dfrobot.com.cn/上都有

![image-20210407113223546](C:\Users\梁庭锋\AppData\Roaming\Typora\typora-user-images\image-20210407113223546.png)



2、程序截图：

![image-20210407112452972](C:\Users\梁庭锋\AppData\Roaming\Typora\typora-user-images\image-20210407112452972.png)

![image-20210407112514798](C:\Users\梁庭锋\AppData\Roaming\Typora\typora-user-images\image-20210407112514798.png)

3、代码：

```c
/*!
 * MindPlus
 * mpython
 *
 */
#include <MPython.h>
#include <DFRobot_Iot.h>
// 函数声明
void obloqMqttEventT0(String& message);
void obloqMqttEventT1(String& message);
// 静态常量
const String topics[5] = {"D46IXy_Gg","qIbXXs_Gg","","",""};
const MsgHandleCb msgHandles[5] = {obloqMqttEventT0,obloqMqttEventT1,NULL,NULL,NULL};
// 创建对象
DFRobot_Iot myIot;


// 主程序开始
void setup() {
	mPython.begin();
	myIot.setMqttCallback(msgHandles);
	myIot.wifiConnect("602iot", "18wulian");
	display.setCursorLine(1);
	display.printLine("WiFi连接中……");
	while (!myIot.wifiStatus()) {yield();}
	display.fillInLine(1, 0);
	display.setCursorLine(1);
	display.printLine("WiFi连接成功！");
	myIot.init("iot.dfrobot.com.cn","Fqqv_ylMR","","Kq3DlylMRz",topics,1883);
	myIot.connect();
	while (!myIot.connected()) {yield();}
	display.setCursorLine(2);
	display.printLine("MQTT连接成功！");
}
void loop() {
	if ((buttonA.isPressed())) {
		display.fillInLine(3, 0);
		display.setCursorLine(3);
		display.printLine("A已按下");
		myIot.publish(topic_0, "亮红灯");
	}
	if ((buttonB.isPressed())) {
		display.fillInLine(3, 0);
		display.setCursorLine(3);
		display.printLine("B已按下");
		myIot.publish(topic_1, "亮蓝灯");
	}
}


// 事件回调函数
void obloqMqttEventT0(String& message) {
	display.setCursorLine(4);
	display.printLine(message);
	rgb.write(-1, 0xFF0000);
}
void obloqMqttEventT1(String& message) {
	display.setCursorLi![image-20210407173234026](C:\Users\梁庭锋\AppData\Roaming\Typora\typora-user-images\image-20210407173234026.png)ne(4);
	display.printLine(message);
	rgb.write(-1, 0x0000FF);
}
```

## 五、测试/调试及实验结果分析

实验结果：

（1）连接WiFi：

![image-20210407173245905](C:\Users\梁庭锋\AppData\Roaming\Typora\typora-user-images\image-20210407173245905.png)

（2）WiFi连接成功、MQTT连接成功：

![image-20210407173423510](C:\Users\梁庭锋\AppData\Roaming\Typora\typora-user-images\image-20210407173423510.png)

（3）按下A，红灯亮：

![image-20210407173504057](C:\Users\梁庭锋\AppData\Roaming\Typora\typora-user-images\image-20210407173504057.png)

（4）按下B，蓝灯亮：

![image-20210407173541625](C:\Users\梁庭锋\AppData\Roaming\Typora\typora-user-images\image-20210407173541625.png)

（5）Easy IoT上MQTT消息详情：

Topic_0：

![image-20210407174229463](C:\Users\梁庭锋\AppData\Roaming\Typora\typora-user-images\image-20210407174229463.png)

Topic_1：

![image-20210407174417022](C:\Users\梁庭锋\AppData\Roaming\Typora\typora-user-images\image-20210407174417022.png)

## 六、实验结论与体会

​	本次实验主要是实现mind+下Easy IoT上mqtt消息的通讯。实验进行得比较成功，主要是听老师讲解实验过程和原理，然后自己跟着动手做起来。有不懂的地方再问老师。

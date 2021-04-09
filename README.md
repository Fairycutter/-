
代码：

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
	display.setCursorLine(4);
	display.printLine(message);
	rgb.write(-1, 0x0000FF);
}
```

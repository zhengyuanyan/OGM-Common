### 扩展的“运行中”状态

扩展的“运行中”模式提供关于设备状态的附加信息。
状态不再以单个位（DPT-1）传输，而是以一个字节（DPT-5）传输。
扩展状态不仅周期性发送，在发生变化时也会发送——因此网络故障或过温等问题可以立即被报告。
通过位掩码可以有选择地评估各种状态信息。

结构：`0b NRRR_TWSB`

* 位 **B**（`1`）表示普通的“运行中”信号（始终有效）。
* 位 **S**（`2`）表示启动过程，在启动延时结束后发送一次。
* 位 **W**（`4`）表示由看门狗引起的重启，仅在同时发送 **S** 位时发送一次。
* 位 **T**（`8`）表示 BCU 的过温报警。
* 位 **R**（`16`）预留给将来的用途。
* 位 **R**（`32`）预留给将来的用途。
* 位 **R**（`64`）预留给将来的用途。
* 位 **N**（`128`）表示是否存在网络连接。

**注意：** 在向设备传输新固件时，某些情况下会出现“由看门狗引起重启”的标志被置位的情况。

**提示：** 如有需要，可以用逻辑模块从中生成单独的 1 位 KO。相应的示例可以通过配置传输导入，然后通过输入 2 进行调整。

```
OpenKNX,cv1,*/LOG/*§f~Name=Bit%20aus%20erweitertem%20Betrieb%20ausmakieren§f~Logic=1§f~Calculate=1§f~Trigger=1§f~TriggerE1=1§f~NameInput1=Erweiterter%20Betriebsstatus§f~E1=1§f~E1Dpt=2§f~E1OtherKO:2=1§f~E1UseOtherKO=1§f~E1LowDpt5:1=0§f~NameInput2=Bitmaske%20(dezimal)§f~E2ConvertInt=5§f~E2=1§f~E2Dpt=2§f~E2LowDpt5Fix=128§f~NameOutput=ausmaskiertes%20Bit§f~OOn=8§f~OOnAll=8§f~OOnFunction=9§>Wert für Eingang 2 passend setzen!§;OpenKNX
```


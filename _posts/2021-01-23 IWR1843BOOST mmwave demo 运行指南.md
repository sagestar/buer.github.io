# 20210123 IWR1843BOOST mmwave demo 运行指南



## 硬件环境

IWR1843BOOST评估版

5V5A电源

macroUSB数据线

## 软件环境

1. mmWave SDK
2. Code Composer Studio（XDS Emulation Software也可以，但我没用）
3. Uniflash

## 操作流程

### 烧写mmv demo到AWR1843BOOST

1. 插电，接线。主要是设置COM3和COM4。

   ![img](https://mkdimg.bueryo.com/img/202012221362.jpeg)![img](https://mkdimg.bueryo.com/img/202012221363.png)

   2. 运行Uniflash，点击选择烧录的芯片，例如AWR1843BOOST。

      ![img](https://mkdimg.bueryo.com/img/202012221364.png)

   3. 点击Setting&Uitlities选项，配置正确的串口（设置下图中显示为XDS110 Class Application/User UART的串口号，这里是COM4）。

      ![img](https://mkdimg.bueryo.com/img/202012221365.png)

   4. 返回Program点击Browse，选择在目录mmWaveSDK\mmwave_sdk_03_04_00_03\packages\ti\demo\xwr18xx\mmw下，文件名为xwr18xx_mmw_demo.bin的bin文件（根据毫米波评估板的不同，所选的文件夹/文件名也会有所变化）。![img](https://mkdimg.bueryo.com/img/202012221366.png)

   5.  点击Load Image进行烧写，等待一段时间。可以在Console控制台窗口可以看到成功的log打印。

      ![img](https://mkdimg.bueryo.com/img/202012221367.png)

   6. 关闭uniflash工具软件。断开AWR1843BOOST电源，将AWR1843BOOST的SOP设置为SOP0=1（短接），其他为0，如下图所示。

      ![img](https://mkdimg.bueryo.com/img/202012221368.jpeg)

      

   ### 运行demo

   a. 上电之前确认AWR1843BOOST的SOP0设置为1，其余为0。接上USB连接，给板子上 电。

   b. 使用浏览器打开https://dev.ti.com/gallery/view/mmwave/mmWave_Demo_Visualizer/ver/3.4.0/，进入mmWave_Demo_Visualizer界面。浏览器建议使用Chrome。如果打不开网页，请查看Q&A，使用离线版Visualizer。

   请注意，板上烧写的mmw demo所在的mmwave sdk版本，要和visualizer的版本匹配。

   你也可以通过访问https://dev.ti.com/gallery，搜索mmWave_Demo_Visualizer关键字，找到对应的，或者最新的visualizer，点击进入网页。

   c. 等待界面加载出来后，点击Options选项下的Serial Port，会提示插件的安装，如下图。根据提示步骤完成安装（需要网络翻墙）。如果无法安装插件，请查看Q&A，使用离线版Visualizer。离线版Visualizer的界面刷新速度较网页版要慢一些。![img](https://mkdimg.bueryo.com/img/202012221369.png)

   d. 插件安装完成后再次点击Options选项下的Serial Port进行串口配置。分别对应于下图的UART PORT和DATA PORT的串口号。波特率不需要做更改。确保对应的端口号设置正确，如图。

   ![img](https://mkdimg.bueryo.com/img/202012221370.png)

   串口设置成功后在网页左下角状态信息会显示Connected，如图。

   ![img](https://mkdimg.bueryo.com/img/202012221371.png)                   

   e. 设置完串口后，在Setup Details选择硬件平台类型。在Scene Slection中可以根据应用场景设置相关参数。确定后点击SEND CONFIG TO MMWAVE DEVICE，系统会自动计算参数，并且把参数发给雷达芯片。在Console Messages串口，你可以看到系统下发的所有参数和命令，如下图所示。

   ![img](https://mkdimg.bueryo.com/img/202012221372.png)

   f. 点击Plots，你可以看到当前demo的输出效果。![img](https://mkdimg.bueryo.com/img/202012221373.jpeg)

   g. 也可以手动加载配置文件。点击上图中红框里的LOAD CONFIG FROM PC AND SEND按钮，选择下发的配置文件，例如C:\ti\mmwave_sdk_03_04_00_03\packages\ti\demo\xwr18xx\mmw\profiles\profile_2d.cfg。

   ## Q&A

   离线版Visualizer使用

   1.在网页https://dev.ti.com/gallery里搜索mmWave_Demo_Visualizer关键字，并且下载相关安装文件。鼠标放在搜索到的visualizer信息框下方的下载箭头上就可以看到下图的下载信息。![img](https://mkdimg.bueryo.com/img/202012221374.png)

   2.在windows下载安装的时候，会出现需要安装GUI Composer Runtime的提示，如果可以在线安装，请选择download from web。或者在第1步中下载好GUI Composer Runtime的安装包，选择从文件安装，如图。![img](https://mkdimg.bueryo.com/img/202012221375.png)

   3.安装成功后，启动软件，你会看到和网页版visualizer类似的界面，其他操作设置同网页版Visualizer。

> AWR1843BOOST mmw demo运行指南(上) http://bbs.eeworld.com.cn/thread-1149506-1-1.html
>
> AWR1843BOOST mmw demo运行指南(下) http://bbs.eeworld.com.cn/thread-1149507-1-1.html
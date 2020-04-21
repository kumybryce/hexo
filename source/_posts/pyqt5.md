---
title: 用pyqt5完成python环境下的GUI设计以及cv2相关操作
date: 2020-4-15 16:52:24
updated: 2020-4-15 16:52:24
hide: true
tags:
  - python
  - opencv
  - Qt
categories:
  - 编程
---

## pyqt5环境配置

 - 我使用的是pycharm+Ubuntu16.04，网上关于环境配置的博客很多，我也是参考了别人的博客，这里简单写一下
 - 第三方库下载，在终端使用pip安装即可，由于我使用`anaconda`创建的虚拟环境`pytorch_yolo3`，所以先激活环境
```bash
	conda activate pytorch_yolo3 
	pip install pyqt5
	pip install pyqt5-tools
```
 - 建立pyqt5和pycharm的连接
 在`pycharm`中，点击`file->Settings->Tools->External Tools`,然后点击`+`，之后的界面如下，名字可自取，`Groups`也可以写成`Qt`，这样之后点开`External Tools`时就会有一个文件夹`Qt`，里面包含了这里建立的工具。`Programs`是指这个工具的位置,在linux系统下，程序的路径为`/usr/lib/x86_64-linux-gnu/qt5/bin/designer`，其余按照图中设置即可
![QtDesiner](https://img-blog.csdnimg.cn/20200415232455901.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY3MjgyOA==,size_16,color_FFFFFF,t_70)`QtDesigner`这个工具可以很方便地设计GUI以及布局等等，它会生成一个`.ui`文件，要使用这个窗口，还需要将这个文件编译成`.py`文件，这个工具就是`PyUIC`，它建立的过程和上面类似，再点击`+`号，`Program`填写你当前的Python编译器所在路径，`Arguments`填写`houtExtension$.py`，其他和图中填写一致即可
![PyUIC](https://img-blog.csdnimg.cn/20200415233119335.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY3MjgyOA==,size_16,color_FFFFFF,t_70)
## pyqt5的使用
 - 创建UI以及转换为py文件
 在`pycharm`下新建一个`.py`文件，然后点击菜单栏中的`tools->External-Tools->QtDesigner`,然后就会弹出`designer`这个强大的工具了，具体怎么设计大家可自行另外学习，挺简单的，傻瓜式操作，尤其要注意的是各个控件的对象名，这个会在Python程序中着重使用到，我的设计如下图所示：左上角区域是两个按键控制程序、两个文本框显示信息，一个`label`用于加载图标，因为这个项目是做安全帽检测且面向应用的，所以当检测到有人没有佩戴安全帽时，加载警告图标到这个`label`上；左下角是一个`label`，它的作用是显示摄像头中的原始数据流，二右边的label则是显示检测完成后的带框的数据流，设计过程中使用了样式表，让空间边框颜色和半径有所改变，以及按键在鼠标悬停和按下时的背景颜色做出改变，设计完成后，保存，文件名为`GUI.ui`，放在Python工程文件夹下，将ui文件拖入`pycharm`中，然后在该文件窗口右键选择`External-Tools->PyUIC`，成功运行后就会产生`GUI.py`，打开这个文件会发现里面有一个`Ui_MainWindow`类，这个就是后面构建窗口用到的类了，要修改布局则需要在`designer`中做出修改，然后保存，重复`PyUIC`的步骤。
 <img src="https://img-blog.csdnimg.cn/20200415233914932.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY3MjgyOA==,size_16,color_FFFFFF,t_70" height="350" width = "720">
 - 在Python中实现逻辑功能
 	在`pycharm`中新建一个Python文件，导入必要的库以及刚才建立的窗口
```python
	from PyQt5 import QtCore, QtGui, QtWidgets
	from PyQt5.QtCore import *
	from PyQt5.QtWidgets import QFileDialog, QMessageBox, QDockWidget, QListWidget
	from PyQt5.QtGui import *
	from GUI import Ui_MainWindow
```
 		
 - 然后定义自己的类
```python
	class mywindow(QtWidgets.QMainWindow, Ui_MainWindow):   #窗口类
	    def __init__(self):
	        super(mywindow, self).__init__()
	        self.setupUi(self)
	        self.setWindowIcon(QIcon("helmet-detection.png"))   #设置窗口程序图标
	        self.setWindowTitle("安全帽检测 v1.2")    #设置窗口程序标题
	        self.cap_button.clicked.connect(self.cap_button_function)   # 打开摄像头按键槽连接，cap_button是按键对应的对象名
        	self.det_button.clicked.connect(self.det_button_function)  # 安全帽检测按键槽连接
       	def cap_button_function(self):
       		#要做的事
       	def det_button_function(self):
       		#要做的事
```
 - 类中首先是初始化函数，这个函数可以做一些界面初始化工作以及各种变量和槽连接，上面的代码中我设置了窗口的图标和标题，以界面中左上角的两个按钮为例，我把它们的对象名取为`cap_button`和`det_button`，槽连接就是当按下这个按钮时就会触发槽函数，上面的槽函数分别为`cap_button_function`和`det_button_function`。`pyqt5`的简单应用记录到此，后面记录多线程等坑。

## pyqt5中的多线程

 - 本项目要实现的功能是读取摄像头数据，运行`yolov3`模型的目标检测算法，然后将检测结果显示出来，其中，算法是非常耗费资源的，之前在单独做目标识别时只使用了`opencv`，直接将检测结果显示出来，而显示窗口的时候其实`opencv`是使用了多线程，第一次将代码移植到`pyqt5`中时，发生了一些有趣的事，当使用`cv2.imshow()`显示检测结果时，Qt窗口程序运行正常，显示也正常，当注释掉`cv2.imshow()`时，程序就卡死了，其实分析一下原因很简单，在主线程中执行如此耗费硬件资源的算法时，主线程就会被阻塞，程序不死才怪，这也是我猜想`opencv`显示图形窗口使用了多线程的原因。好了，要解决这个问题，就要使用`pyqt5`中的多线程。
 1. 线程划分
 根据要实现的任务，我将线程划分为4个，一旦开启摄像头，就必须不间断地读取视频流，不管检测算法是否运行所以`读取视频流`为第一个线程，`运行检测算法`自然而然地成为了第二个线程，刷新界面上的图像其实可以使用定时器，每30ms刷新一次即可，而计时器对应的响应函数运行是也是多线程的。所以一共有4个线程。
 2. pyqt5多线程的实现
 和`C++`中一样，要实现Qt中的多线程，要定义自己的线程类，这个线程类继承于`QThread`，所以先定义自己的线程类
```python
	class mythread(QThread):    #执行yolo算法的线程，算法和显示是在不同的线程中执行的，以防止主线程拥塞
	    def __init__(self, parent=None):
	        super(mythread, self).__init__()
	
	    def run(self):
	        YOLO()
```

- 这是第一个线程类，你需要做的就是在`run()`函数下写入你想这个线程做的事，我把线程类和窗口类的定义放在了同一个文件下，这样方便使用全局变量，省去了线程间通信的麻烦，然后在窗口类中声明线程变量，在需要开启此线程时，`start()`一下就好了。
```python
	self.sub_thread = mythread()   # 实例化yolo算法线程
	self.sub_thread.start() # 开始yolo算法线程
```

- 那么其他线程呢？很简单，再构造一个类，重复上面的操作就可以了
3. 优雅地结束子线程
子线程在运行时是无法指定父对象的，所以主线程结束（也就是窗口关闭）时，子线程资源并不会被回收，所以会报错，优雅地结束子线程关闭窗口才会以`code 0 `结束程序。要优雅地结束子线程，就要在关闭窗口时触发的函数中做手脚，这个函数就是`closeEvent()`：
```python
	    def closeEvent(self, event):    #关闭窗口响应函数，在这里面弹出确认框，并销毁线程
	        global cap,det_flag
	        self.timer_camera_raw.stop()
	        self.timer_camera_det.stop()
	        reply = QtWidgets.QMessageBox.question(self,
	                                               '本程序',
	                                               "是否要退出程序？",
	                                               QtWidgets.QMessageBox.Yes | QtWidgets.QMessageBox.No,
	                                               QtWidgets.QMessageBox.No)
	        if reply == QtWidgets.QMessageBox.Yes:
	            cap.release()
	            event.accept()
	            os._exit(0)
	        else:
	            event.ignore()
```

- 如上所示，结束时先暂停显示弹出确认关闭窗口，确认后先释放摄像头资源，再处理子线程，这个方法是参考别人的博客写的，具体原理我也不是很懂，之前在C++中实现时在这个函数中写的是
```cpp
	sub_thread.quilt();
	sub_thread.wait();
```

## opencv图片叠加混合
- 之所以用到这个，其实是之前想在检测到危险时在输出图片上加上警示标志，这个在opencv中的操作在这里记录一下，注释很详细，就不介绍了
```python
	rows, cols, channels = warning_img.shape    #获取警告标志形状
	roi = img[0:rows, 0:cols]   #roi是图像操作的区域
	gray = cv2.cvtColor(warning_img, cv2.COLOR_BGR2GRAY)    #灰度化警告标志
	ret, mask = cv2.threshold(gray, 70, 255, cv2.THRESH_BINARY_INV) #二值化警告标志生成遮罩
	mask_inv = cv2.bitwise_not(mask)    #遮罩反转，遮罩用来操作背景图片，将标志要显示的部分的像数值置为0，相当于抠出来，而反转遮罩用来将标志周边不显示的部分去掉
	img1_bg = cv2.bitwise_and(roi, roi, mask=mask)  #比特运算，抠图
	img2_fg = cv2.bitwise_and(warning_img, warning_img, mask=mask_inv)
	dst = cv2.add(img1_bg, img2_fg) #执行加法
	img[0:rows, 0:cols ]=dst    #将图像操作区域放入检测输出的图中
```

## 同时播放两种声音
- 我的构思是当检测到危险时发出警报声和语音提示声，这也是实际应用中需要的，这里使用的是pygame的两个发声模块，先初始化和载入声音：
```python
	import pygame   #用来发声音提示
	pygame.mixer.init() #声音输出初始化
	track = pygame.mixer.music.load("beep.wav") #载入警报声
	audio = pygame.mixer.Sound("audio.wav") #载入语音提示
```

- 其实在实际使用的时候`pygame.mixer.Sound()`这个方法老是出错，最终找到问题所在是因为这个模块不支持`mp3`格式的声音，而第一个是可以载入`mp3`的。在具体使用时用下面的语句，先判断声源是否在播放，否则会变成鬼畜。
```python
    if pygame.mixer.music.get_busy()==False:    #如果当前没有播放音效，则播放，否则一直检测到危险时，警报将变成鬼畜
        pygame.mixer.music.play()
        audio.play()
    if stop_music == True:  #这里是按下了暂停检测键，把音乐停掉，这个音乐会先于yolo()函数暂停，因为这个标志是在鼠标按下时有效的，而暂停检测标志是在鼠标释放的时有效的候
        pygame.mixer.music.stop()
        audio.stop()
```

## 设置文本框只能输入数字
- 这是因为我设置了一个可以改变置信度阈值的文本框，必须限制其输入内容以及输入范围和小数点位数，这在Qt里面是非常容易实现的，记录一下：
```python
	doubleValidator = QDoubleValidator()
	doubleValidator.setRange(0.1, 0.9)
	doubleValidator.setNotation(QDoubleValidator.StandardNotation)
	doubleValidator.setDecimals(2)# 设置精度
	self.lineEdit_thresh.setValidator(doubleValidator)
```

## 程序功能介绍
- 程序界面

<img src="https://img-blog.csdnimg.cn/20200416010210656.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY3MjgyOA==,size_16,color_FFFFFF,t_70"  height="350" width="720">

- 点击`开启摄像头`将打开摄像头或者被测试的视频，并在`源视频显示窗口`显示当前画面，摄像头每30ms读取一诊，显示则是30ms刷新一次。点击`安全帽检测`按钮，将开始载入模型并进行安全帽检测，检测结果将输出到`检测输出视频显示窗口`，由于检测算法耗费硬件资源，根据GPU性能的好坏，检测时视频帧率有所不同，所以在右上角区域显示了`当前帧率`，为了方便调试和得到更好的效果，在右上角设置了改变置信度阈值的功能。输入`0.10-0.90`的数字，点击`确认修改`即可，当检测到有人没有佩戴安全帽时，程序会发出警报声并提示佩戴安全帽，同时左上角会有文字提示和图标警告，绿色图标表示安全，红色图标则表示有潜在危险。

<img src="https://img-blog.csdnimg.cn/20200416011542345.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY3MjgyOA==,size_16,color_FFFFFF,t_70"  height="350" width="720">
<img src="https://img-blog.csdnimg.cn/2020041601161746.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY3MjgyOA==,size_16,color_FFFFFF,t_70"  height="350" width="720">

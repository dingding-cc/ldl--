# 学习c语言并自制操作系统

![自制操作系统小组logo](http://cqkj-dingding-monitor.oss-cn-hangzhou.aliyuncs.com/img/自制操作系统小组logo.jpg)

## 前言

其灵感来源于日本人川合秀实的自制osaka系统，具体可参考其作品《30天自制操作系统》一书

```html
https://baike.baidu.com/item/osask/2976322
```

本项目的初衷就是想在此基础上简化操作系统制作流程，使其适用于大众学习了解，此其一；另外，为了创作出适合具体开放环境的操作系统。如果从兴趣的角度讲，可以供单片机爱好者适用，也契合当下的元宇宙理念以及即将普及的智慧家居。

## 《30天自制操作系统》简要内容

### 对于需要开发的主要内容做如下简单说明

| <font size=5>时间</font> |              <font size=5>需要完成的内容</font>              |
| ------------------------ | :----------------------------------------------------------: |
| DAY1                     | 使用汇编完成了 helloos.nas ,经过 asm 生成二进制文件 helloos.img, 使用QEMU软件模拟PC输出hello world 。 |
| DAY2                     | 加入makefile。制作启动区。 汇编写出 helloos.nas , asm 后输出 ipl.bin/ipl.lst, 用磁盘映像管理工具 edimg.exe 生成helloos.img. |
| DAY3                     | 制作启动区（从软盘读取数据填充到内存的对应位置），汇编写出一个简易的操作系统（黑屏），用c语言写出停止函数,用汇编写出停止的底层实现，用作者实现的编译器编译后三者链接后输出二进制磁盘映像交由系统启动。 |
| DAY4                     | 用汇编实现对显存和中断标志的io，C语言实现绘图，调色函数，在main函数中实现绘制系统框架图。编译链接后交由系统运行。 |
| DAY5                     | 用 struct （结构体） 存储显存信息，加入字体文件hankaku.txt，C语言绘制鼠标图形，加入字符显示函数，引入GDT（全局段号记录表）和IDT（中断记录表），利用结构体和C语言实现读写 GDT 和 IDT 。 |
| DAY6                     | 将源文件分割成多个文件，重复部分整理至共有的头文件中。利用通配符简化makefile，设定PIC（可编程中断控制器），C语言实现鼠标键盘中断处理函数，汇编将中断函数处理包装（中断返回，中断标志置位）后注册至IDT。 |
| DAY7                     | 完善键盘中断处理，加入键盘输入缓存（FIFO BUF），利用环形队列实现结构体 KEYBUF， 整理至 FIFO.C 文件中；加入鼠标控制电路初始化代码，加入鼠标数据取得方法与中断反馈代码（和键盘类似）。 |
| DAY8                     | 加入鼠标数据处理与解读（读取鼠标数据【3连字节】，验证，转换为坐标/状态/按钮值），鼠标移动（频繁擦除重画鼠标ico）。解释了asmhead.nas头文件内容。 |
| DAY9                     | 对源文件分割整理，加入高速缓存检测，内存检测，内存管理（将每一段空闲内存记录在特定内存空间中并用结构体存储记录。） |
| DAY10                    | 增强内存管理功能（完善memman_alloc 和 memman_free ，以大区块进行内存分配和释放）。加入叠加处理（加入图层），图像刷新（从底向高层依次绘制图层），图层移动，图层释放，范围刷新技术（以提高图像绘制的速度）。 |
| DAY11                    | 支持鼠标移出屏幕（解决刷新溢出），改善图层修改函数，绘制窗口图像，利用制作图层map消除刷新闪烁。 |
| DAY12                    | 定时器设置，设置PIT(可编程定时器)，设置中断，定时器函数，管理定时器，加快优化定时器中断处理速度。3秒和10秒时输出屏幕。 |
| DAY13                    | 整理简化字符串显示函数，利用定时器改善和调整FIFO缓冲区，定时器性能测试，用链表结构管理定时器，增加定时器哨兵。 |
| DAY14                    | 改进性能测试，高分辨率支持（需要转换到真机上运行），键盘输入，输入输出至窗口。制作字符框，鼠标拖动窗口功能。 |
| DAY15                    | 多任务处理，定义任务状态段，任务切换功能，测试多任务运算速度，运行测试（让TASK A 和 Task B 轮流数数）。 |
| DAY16                    | 任务管理（用结构体记录和管理任务），任务的休眠与唤醒，增加窗口数量（每一任务一个窗口），任务优先级设置。 |
| DAY17                    | 对闲置任务的管理（把闲置任务置于最底层），制作命令行窗口，窗口切换（设定为TAB按键），命令行接受键盘数据。符号的输入（感叹号和百分号），实现大小写输入（按键编码与字符编码转换表），对各种锁定键的支持，点亮和熄灭键盘指示灯。 |
| DAY18                    | 控制焦点窗口的光标闪烁，命令行支持回车键，对窗口滚动的支持，编写与实现mem,dir命令。 |
| DAY19                    | 加入type命令（就是linux中的cat命令），支持FAT(文件分配表)，写出第一个系统程序（让电脑死机。。。）。 |
| DAY20                    |    整理程序，制作系统应用程序接口（API），显示字符的API。    |
| DAY21                    |  保护操作系统关键段，用C语言写作操作系统API，对异常的支持。  |
| DAY22                    | 加强系统保护（防止API被修改），对于程序中溢出的异常处理，强制结束键（强制结束死循环的程序），C语言显示字符串API，窗口显示API。 |
| DAY23                    | 重写malloc的API,加入对窗口中图形绘制的API（点，指点，窗口，刷新与关闭窗口）,键盘输入API，强制结束后关闭窗口，WALK小游戏（能控制在窗口上下左右移动的点）. |
| DAY24                    | 窗口切换（使用按键F11，或鼠标点击），鼠标拖动窗口移动，关闭窗口，定时器API。 |
| DAY25                    | 蜂鸣器（需要真机运行），增加调色盘（把颜色API改为256色），改进256色至真彩色，支持同时开启多个（最多10个）命令行窗口（新窗口继承原窗口变量，重新分配内存，和fork有点类似），去掉开机自带的小窗口。 |
| DAY26                    | 提高窗口拖动速度，启动时打开一个命令行窗口（编写开命令行窗口函数，用shift + F2，打开新命令行窗口），取消命令行数量限制。关闭命令行后进行内存回收。ncst命令（启动程序不打开命令行窗口），start命令（打开一个新的命令行窗口）。 |
| DAY27                    |   修复点“x”无法关闭窗口的小bug，保护应用程序，整理源代码。   |
| DAY28                    | alloca(对esp做减法的函数)，文件操作API,命令行API，日文的文字显示。 |
| DAY29 & DAY30            | 这两天都是在做应用程序，主要是代码，讲解很少，就合起来写了，notrec(非矩阵窗口)，bball(画球)，invader(外星人游戏,就是小蜜蜂），tek_getseze & tek_decomp(文件压缩，制作成tek格式),calc(计算器),tview(文本阅览),mmlplay(音乐播放器),gview(图片阅览). |




# :octcat:PythonGame仓库

学了几天Python，学了GUI库、Pygame库，又做一些游戏了，有贪吃蛇:snake:、滑雪:snowflake:和AI对下五子棋:black_square_button:

GIthub仓库：<https://github.com/Github-Programer/PythonGame>

## :dog:客官，点个赞？

:star:如果觉得对您有帮助的话，点个 star ，再走？



## :cat:详细解释

首先，需要几个库，打开cmd，输入如下命令（如果已经有了，那么就不用了）

```powershell
pip install pygame
```

安装pygame，如果出错，可以试试

```powershell
pip3 install pygame
```

### :bug:1$贪吃蛇

我做了一个手动版本，和一个AI版本，AI就是自动寻径原理，自动的，知道框满了不会碰边，但是由于算法不够精良（BFS广搜），所以蛇越长速度越慢……:turtle:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200503125240496.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Nvb2w5OTc4MQ==,size_16,color_FFFFFF,t_70)
**:calendar:主要思路**
:hourglass_flowing_sand:（1）蛇每走一步，就使用BFS计算游戏界面中每个位置（蛇身除外）到达食物的最短路径长；

:hourglass_flowing_sand:（2）将蛇的安全定义为蛇是否可以跟着蛇尾运动，即蛇头和蛇尾间是否存在路径；

:hourglass_flowing_sand:（3）蛇每次行动前先利用虚拟的蛇进行探路，若虚拟的蛇吃完食物后是安全的，真蛇才行动；

:hourglass_flowing_sand:（4）若蛇和食物之间不存在路径或者吃完食物后并不安全，就跟着蛇尾走；

:hourglass_flowing_sand:（5）若蛇和食物之间、蛇和蛇尾之间均不存在路径，就随便挑一步可行的来走；

（6）保证目标是食物时蛇走最短路径，目标是蛇尾时蛇走最长路径。
**:calendar:不足之处**

由于食物是随机出现的，若虚拟的蛇跑一遍发现去吃食物是不安全的，真蛇就不会去吃食物，而是选择追着蛇尾跑，若一直如此，就陷入了死循环，蛇一直追着蛇尾跑跑跑。。。

直到你终止游戏为止。。。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200503125434765.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Nvb2w5OTc4MQ==,size_16,color_FFFFFF,t_70)
仓库中还有一个简单版，Normal的，可以手动操作，但是一般走几步就废了，控制不住:sweat:

### :snowman:2$滑雪小游戏
**:calendar:TOOL**
开发工具：Python版本：3.6.4

相关模块：pygame模块；以及一些Python自带的模块。

环境搭建：安装Python并添加到环境变量，pip安装需要的相关模块即可。

**:calendar:游戏规则：**

玩家通过“AD”键或者“←→”操控前进中的滑雪者，努力避开路上的树，尽量捡到路上的小旗。

如果碰到树，则得分减50，如果捡到小旗子，则得分加10。

**:calendar:逐步实现：**

:hourglass_flowing_sand:Step1：定义精灵类

由于游戏涉及到碰撞检测(滑雪者与树和小旗之间的碰撞)，因此我们定义两个精灵类，分别用于代表滑雪者和障碍物(即树和小旗)：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200503130652715.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Nvb2w5OTc4MQ==,size_16,color_FFFFFF,t_70)
其中，滑雪者在前进过程中应当拥有向左，向右偏移的能力，并且在偏移时滑雪者向前的速度应当减慢才更加合乎常理，这样才能供玩家操作。同时，滑雪者应当拥有不同的姿态来表现自己滑行时的状态：

直线：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93d3cudzNjc2Nob29sLmNuL2F0dGFjaG1lbnRzL2ltYWdlLzIwMTgwODA5LzE1MzM4MDc3NTg5NzA5NTMucG5n?x-oss-process=image/format,png)

左偏一点：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93d3cudzNjc2Nob29sLmNuL2F0dGFjaG1lbnRzL2ltYWdlLzIwMTgwODA5LzE1MzM4MDc3NjQyNjUyODYucG5n?x-oss-process=image/format,png)

左偏很多：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93d3cudzNjc2Nob29sLmNuL2F0dGFjaG1lbnRzL2ltYWdlLzIwMTgwODA5LzE1MzM4MDc4MTAyNDc0NzQucG5n?x-oss-process=image/format,png)

右偏一点：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93d3cudzNjc2Nob29sLmNuL2F0dGFjaG1lbnRzL2ltYWdlLzIwMTgwODA5LzE1MzM4MDc4MTQ1ODU3OTUucG5n?x-oss-process=image/format,png)

右偏很多：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93d3cudzNjc2Nob29sLmNuL2F0dGFjaG1lbnRzL2ltYWdlLzIwMTgwODA5LzE1MzM4MDc4MTg1NDA5MzgucG5n?x-oss-process=image/format,png)

另外，尽管滑雪者的左右移动通过移动滑雪者本身实现，但是滑雪者的向前移动是通过移动障碍物实现的。

:hourglass_flowing_sand:Step2：随机创建障碍物

现在我们需要定义一个随机创建障碍物的函数，以便在游戏主循环中调用：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93d3cudzNjc2Nob29sLmNuL2F0dGFjaG1lbnRzL2ltYWdlLzIwMTgwODA5LzE1MzM4MDc4MjIzMDM3NTkuanBn?x-oss-process=image/format,png)

:hourglass_flowing_sand:Step3：游戏主循环

首先我们初始化一些参数：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93d3cudzNjc2Nob29sLmNuL2F0dGFjaG1lbnRzL2ltYWdlLzIwMTgwODA5LzE1MzM4MDc4MjUzMjQ4MzUuanBn?x-oss-process=image/format,png)

其中障碍物创建两次的目的是便于画面衔接。

然后我们就可以定义主循环了：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93d3cudzNjc2Nob29sLmNuL2F0dGFjaG1lbnRzL2ltYWdlLzIwMTgwODA5LzE1MzM4MDc4MzI2NDgxOTIuanBn?x-oss-process=image/format,png)

主循环的内容包括：

事件监听、障碍物的更新、碰撞检测以及分数的展示等内容，总之还是很容易实现的。

:hourglass_flowing_sand:Step4：其他

开始、结束界面这些，就靠大家自己发挥了，我就写了一个简单的开始界面：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93d3cudzNjc2Nob29sLmNuL2F0dGFjaG1lbnRzL2ltYWdlLzIwMTgwODA5LzE1MzM4MDc4MzYxMjk5OTcuanBn?x-oss-process=image/format,png)
### :fries:2$AI五子棋
比较愚蠢的AI五子棋。

T_T当然你好好和它下，它还是比较机智的。

让我们愉快地开始吧~~~

**:calendar:原理简介**

对于五子棋这样的博弈类AI，很自然的想法就是让计算机把当前所有可能的情况都尝试一遍，找到最优的落子点。这里有两个问题：

:hourglass_flowing_sand:（1）如何把所有可能的情况都尝试一遍；

:hourglass_flowing_sand:（2）如何定量判断某落子点的优劣。

对于第一个问题，其实就是所谓的博弈树搜索，对于第二个问题，其实就是所谓的选择评估函数。评估函数的选取直接决定了AI算法的优劣，其形式也千变万化。可以说，每个评估函数就是一个选手，对不同的棋型每个选手自然有不同的看法和应对措施，当然他们的棋力也就因此各不相同了。

但博弈树搜索就比较固定了，其核心思想无非是让计算机考虑当前局势下之后N步所有可能的情况，其中奇数步（因为现在轮到AI下）要让AI方的得分最大，偶数步要让AI方的得分最小（因为对手也就是人类，也可以选择最优策略）。

当然这样的搜索其计算量是极大的，这时候就需要剪枝来减少计算量。例如下图：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93d3cudzNjc2Nob29sLmNuL2F0dGFjaG1lbnRzL2ltYWdlLzIwMTgwODEzLzE1MzQxNDgwMDY0OTk4NzEuanBn?x-oss-process=image/format,png)

其中A代表AI方，P代表人类方。AI方搜索最大值，人类方搜索最小值。因此Layer3的A1向下搜索的最终结果为4，Layer3的A2向下搜索，先搜索Layer4的P3，获得的分值为6，考虑到Layer2的P1向下搜索时取Layer3的A1和A2中的较小值，而Layer3的A2搜索完Layer4的P3时，其值就已经必大于Layer3的A1了，就没有搜索下去的必要了，因此Layer3到Layer4的路径3就可以剪掉了。

上述搜索策略其实质就是：

minimax算法+alpha-beta剪枝算法。

了解了上述原理之后，就可以自己写代码实现了。当然实际实现过程中，我做了一些简化，但万变不离其宗，其核心思想都是一样的。

具体实现过程详见相关文件中的源代码。

**:calendar:缺点**

速度比较慢，棋子越多，反应越慢，所以说很愚蠢，下了30个的时候电脑烫的不行

**:calendar:使用演示**

在cmd窗口运行GobangAI.py文件即可。

下面的视频是我和AI的一局对弈，我执黑先行，所以赢的比较轻松T_T。毕竟五子棋先手者优势巨大，或者说在某些情况/规则下是必胜的。至于原因，在相关文件中提供了两篇论文，感兴趣的可以看看。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200503131714831.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Nvb2w5OTc4MQ==,size_16,color_FFFFFF,t_70)



## 联系方式

+ :mailbox_closed:QQ：3392446642
+ :e-mail:邮箱：<Thomaswang1h@163.com>

 

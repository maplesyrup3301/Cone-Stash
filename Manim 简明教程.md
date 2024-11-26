# 第一部分 安装
最简单的一站式安装方式是使用Chocolatey. 下面我们先安装Chocolatey.

1. 找到Powershell（当程序不在桌面上时记得使用搜索功能）；
2. 右击并以管理员身份打开；
3. 确保`Get-ExecutionPolicy` 不处在 `Restricted` 下，一般处在`Restricted`下的是公用电脑，因此个人电脑上大概率不需要考虑这一步；
4. 运行命令
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
如果没有报错就安装成功啦！

然后要安装Manim只需要使用如下命令：
```powershell
choco install manimce
```
注意运行`choco`命令时也是需要以管理员身份的
# 第二部分 使用
本文假设读者对Python面向对象编程已经熟练掌握；

新建一个项目文件夹，使用编辑器打开文件夹（本文使用VScode）
新建Python文件，复制入如下代码：
```python
from manim import *


class CreateCircle(Scene):
    def construct(self):
        circle = Circle()  # create a circle
        circle.set_fill(PINK, opacity=0.5)  # set the color and transparency
        self.play(Create(circle))  # show the circle on screen
```
Manim渲染的每一个场景都由一个类定义，而每个类中被渲染的方法永远是`construct(self)`，只有其中的内容会被渲染出来
每一个类都是一个**场景**，因此需要继承Manim所定义的**场景类**
其中平面场景常继承`Scene`，三维场景常继承`ThreeDScene`

Manim中的几何对象都是`Mobject`类的子类；一般地说，每个对象的生命周期是：
1. 声明与初始化
2. 进入主场景
3. 执行动画
4. 离开主场景（移除）
在上面的代码中，我们定义了圆类的一个实例`circle`，随后使用方法`set_fill(...)`设置了其填充颜色，最后使用`self.play(Create(circle))`在场景中创建了该圆；
有什么对象可以声明，以及每个对象有什么方法可以使用都可以从文档中查看，因此不赘述
`self.play(...)`方法接受一个动画，并且将其播放出来，因此我们不能直接传入对象，而是要使用一个动画将对象包裹起来，常用的动画有：
1. `Write()` - 写，特别是写字，但是形状也可以使用
2. `Create()` - 创建，比写更通用，在形状上的作用与写基本一致
3. `FadeIn()` - 淡入
4. `Transform()`
5. `ReplacementTransform()`
---
title: "Virtuoso Layout Editor快捷键归纳"
date: 2015-04-04
draft: false
slug: "virtuoso-layout-editor-shortcuts"
categories:
  - ""
---

首先介绍下鼠标操作:  
单击左键选中一个图形(如果是两个图形交叠的话，单击左键选中其中一个图形，再单击选中另一个图形)  
用左键框选，选中一片图形，某个图形要被完全包围才会被选中。  
中键单击调出常用菜单命令(很少用，要点两下，麻烦。我们有快捷键的嘛)   
右键点击拖放用来放大。放大后经常配合F键使用，恢复到全部显示。配合Tab键使用，平移视图。右键还有“Strokes”，就是点住右键画些图线，就能实现调用某些命令。   
Shift+左键加选图形，Ctrl+左键减选图形。(Cadence菜单中大写表示+按shift，Ctrl写成^)   

F1显示帮助窗口。   
F2保存。   
F3这个快捷键很有用，是控制在选取相应工具后是否显示相应属性对话框的。比如在选取Path工具后，想控制Path的走向，可以按F3调出对话框进行设置。  
F4英文是Toggle Partial Select，就是用来控制是否可以部分选择一个图形。   
F5打开。  
F8 Guided Path Create切换至L90XYFirst。  
Ctrl+A全选。这个和windows下是一样的。  
Shift+B Return。这个牵扯到“Hierarchy”。翻译成“等级”。这个命令就是等级升一级，升到上一级视图。   
B键 去某一级(Go to Level)。  
Ctrl+C中断某个命令，不常用。一般多按几次Esc键取消某个命令。   
Shift+C裁切(Chop)。首先调用命令，选中要裁切的图形，后画矩形裁切。   
C键 复制。复制某个图形。   
Ctrl +D取消选择。这个也可用鼠标点击空白区域实现。这个快捷键和Photoshop中的取消选区的快捷键是一样的。还有Shift+D，和D也是取消选择。   
Shift+E和E是控制用户预设的一些选项。  
Ctrl+F显示上层等级Hierarchy。  
Shift+F显示所有等级。  
F键 满工作区显示。就是显示你所画的所有图形。  
Ctrl+G(Zoom To Grid)。  
G这个快捷键是开关引力(Gravity)的。Gravity我觉得和AutoCAD里的吸附Snap差不多，就是会吸附到某些节点上去。有时候这个Gravity是很讨厌的，总是乱吸附，这时可以点击G键关闭Gravity，操作完成后再打开。  
I键 插入模块(Instance)。  
Shift+K清除所有标尺。要清除的话总是要清除所有标尺。   
K键 标尺工具。Ruler  
L键 标签工具。Label。标签要加在特定的text层上。  
Shift+M合并工具。Merge  
M键 移动工具。Move。点选Move工具后，选中要移动的图形，然后在屏幕上任意一处单击一下，这个就是确定移动的参考点，然后就可以自由移动了。这个也可以通过鼠标先选中一个图形，移动鼠标当鼠标箭头变成十字方向的时候就可以拖动来实现。   
Ctrl+N，Shift+N和N是控制走向的。   
Ctrl+N先横后竖。L90XFirst  
Shift+N直角正交。Orthogonal  
N键 斜45对角+正交。Diagonal  
Shift+O旋转工具。Rotate  
O键 插入接触孔。Create Contact   
Ctrl+P插入引脚。Pin  
Shift+P多边形工具。Polygon  
P键 插入Path，翻译成“路径”。有人翻译成“管道”。这些最后都要Convert to Polygon的。  
Shift+Q打开设计属性对话框，选中一个图形先。  
Q键图形对象属性。这个实用。经常用来更改图形属性。也是先选中一个图形。  
Ctrl+R是Redraw重画。  
Shift+R是Reshape重定形。就是在原来的图形上再补上一块图形。  
R键 矩形工具。Rectangle应该是用的最多的工具了吧。  
Ctrl+S是Split，翻译成“添加拐点”，就是配合Stretch命令可以是原来直的Path打弯。  
Shift+S是Search查找。  
S键 拉伸工具。Stretch。要求是框选要拉伸图形，再拉伸。我觉得这个拉伸工具是Virtuso版图设计区别于其他绘图软件的精华所在，能在保持图形原有性质的前提下，自由拉伸。这个符合Layout布局的要求。  
Ctrl+T (Zoom to Set)。  
Shift+T (Tree)，我觉得其实应该叫Hierarchy Tree。  
T键 是Layer Tap，层切换。这个菜单命令中没有。这个快捷键其实挺方便。按过T后点击一个图形，就自动切换到刚刚点击图形的的层上去了。有了这个快捷键就不必频繁点击LSW窗口了。  
Shift+U重复Redo。撤销命令后，再反悔。  
U键 撤销Undo。  
Ctrl+V (Type in CIW)  
V键 关联Attatch。这个命令要解释一下。将一个子图形(child)关联到一个父图形(parent)后。关联后，若移动parent，child也将跟着移动；移动child，parent不会移动。可以将Label关联到Pad上。  
Ctrl+W关闭窗口。关闭窗口的另一种方法。  
Shift+W下一个视图。Next View  
W键 前一视图。Previous View  
Ctrl+X适合编辑。Fit Edit。感觉和F差不多。  
Shift+X下降一等级。Descend   
X键(Edit in Place)。这个比较搞，很难翻译。在Hierarchy菜单下。   
Ctrl+Y叫Cycle Select试了下没成功。  
Shift+Y粘贴Paste。配合Yank使用。   
Y键 区域复制Yank。和Copy是有区别的，Copy只能复制完整图形对象。   
Ctrl+Z视图放大两倍Zoom In by 2  
Shift+Z视图缩小两倍Zoom Out by 2  
Z键 视图放大。  
ESC键Cancel。  
Tab键 平移视图Pan。按Tab，用鼠标点击视图区中某点，视图就会移至以该点为中心。   
Delete键 删除。  
BackSpace键 撤销上一点。这个很有用。就不用因为Path一点画错而删除重画。可以撤销上一点。  
Enter键 确定一个图形的最后一点。也可双击鼠标左键结束。  
Ctrl+方向键 移动Cell。  
Shift+方向键 移动鼠标。每次半个格点的距离。  
方向键 移动视图。  
快捷键除了cadence自带的以外，更多的还是自己写的。只要会写skill程序，就可以自己定义快捷键。其中涉及到对.cdsinit文件的修改。   

联机手册特别提到：不要滥用shift＋1这个bindkey。  

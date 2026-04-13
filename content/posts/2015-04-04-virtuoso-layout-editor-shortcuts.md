---
title: "Virtuoso Layout Editor快捷键归纳"
date: 2015-04-04
draft: false
slug: "virtuoso-layout-editor-shortcuts"
categories:
  - ""
---

## 鼠标操作

- **单击左键**：选中一个图形（如果两个图形交叠，单击选中其中一个，再单击选中另一个）
- **左键框选**：选中一片图形，某个图形要被完全包围才会被选中
- **中键单击**：调出常用菜单命令（很少用，要点两下。我们有快捷键的嘛）
- **右键点击拖放**：放大。放大后经常配合F键使用，恢复到全部显示。配合Tab键使用，平移视图。右键还有"Strokes"，就是点住右键画些图线，就能实现调用某些命令
- **Shift+左键**：加选图形
- **Ctrl+左键**：减选图形（Cadence菜单中大写表示+按Shift，Ctrl写成^）

## 快捷键

| 快捷键 | 功能 |
|--------|------|
| F1 | 显示帮助窗口 |
| F2 | 保存 |
| F3 | 控制在选取相应工具后是否显示相应属性对话框。比如在选取Path工具后，想控制Path的走向，可以按F3调出对话框进行设置 |
| F4 | Toggle Partial Select，控制是否可以部分选择一个图形 |
| F5 | 打开 |
| F8 | Guided Path Create切换至L90XYFirst |
| Ctrl+A | 全选（和Windows下一样） |
| Shift+B | Return（等级升一级，升到上一级视图） |
| B | 去某一级（Go to Level） |
| Ctrl+C | 中断某个命令，不常用。一般多按几次Esc键取消某个命令 |
| Shift+C | 裁切（Chop）。首先调用命令，选中要裁切的图形，后画矩形裁切 |
| C | 复制某个图形 |
| Ctrl+D | 取消选择（也可用鼠标点击空白区域实现，和Photoshop中的取消选区快捷键一样）。Shift+D和D也是取消选择 |
| Shift+E / E | 控制用户预设的一些选项 |
| Ctrl+F | 显示上层等级Hierarchy |
| Shift+F | 显示所有等级 |
| F | 满工作区显示（显示所画的所有图形） |
| Ctrl+G | Zoom To Grid |
| G | 开关引力（Gravity）。Gravity和AutoCAD里的吸附Snap差不多，会吸附到某些节点上去。有时候Gravity很讨厌，总是乱吸附，这时可以点击G键关闭Gravity，操作完成后再打开 |
| I | 插入模块（Instance） |
| Shift+K | 清除所有标尺 |
| K | 标尺工具（Ruler） |
| L | 标签工具（Label）。标签要加在特定的text层上 |
| Shift+M | 合并工具（Merge） |
| M | 移动工具（Move）。点选Move工具后，选中要移动的图形，然后在屏幕上任意一处单击确定移动的参考点，然后自由移动。也可以通过鼠标先选中图形，移动鼠标当箭头变成十字方向时拖动 |
| Ctrl+N | 先横后竖（L90XFirst） |
| Shift+N | 直角正交（Orthogonal） |
| N | 斜45对角+正交（Diagonal） |
| Shift+O | 旋转工具（Rotate） |
| O | 插入接触孔（Create Contact） |
| Ctrl+P | 插入引脚（Pin） |
| Shift+P | 多边形工具（Polygon） |
| P | 插入Path（路径/管道）。这些最后都要Convert to Polygon |
| Shift+Q | 打开设计属性对话框（选中一个图形先） |
| Q | 图形对象属性。经常用来更改图形属性，先选中一个图形 |
| Ctrl+R | Redraw重画 |
| Shift+R | Reshape重定形。在原来的图形上再补上一块图形 |
| R | 矩形工具（Rectangle），应该是用的最多的工具 |
| Ctrl+S | Split（添加拐点），配合Stretch命令使原来直的Path打弯 |
| Shift+S | Search查找 |
| S | 拉伸工具（Stretch）。要求是框选要拉伸图形再拉伸。这是Virtuoso版图设计区别于其他绘图软件的精华所在，能在保持图形原有性质的前提下自由拉伸 |
| Ctrl+T | Zoom to Set |
| Shift+T | Tree（Hierarchy Tree） |
| T | Layer Tap（层切换）。按过T后点击一个图形，就自动切换到刚刚点击图形的层上去了，不必频繁点击LSW窗口 |
| Shift+U | 重复（Redo）。撤销命令后再反悔 |
| U | 撤销（Undo） |
| Ctrl+V | Type in CIW |
| V | 关联（Attach）。将一个子图形（child）关联到一个父图形（parent）后，若移动parent，child也将跟着移动；移动child，parent不会移动。可以将Label关联到Pad上 |
| Ctrl+W | 关闭窗口 |
| Shift+W | 下一个视图（Next View） |
| W | 前一视图（Previous View） |
| Ctrl+X | Fit Edit（适合编辑，感觉和F差不多） |
| Shift+X | 下降一等级（Descend） |
| X | Edit in Place（Hierarchy菜单下，比较难翻译） |
| Ctrl+Y | Cycle Select |
| Shift+Y | 粘贴（Paste），配合Yank使用 |
| Y | 区域复制（Yank）。和Copy有区别，Copy只能复制完整图形对象 |
| Ctrl+Z | 视图放大两倍（Zoom In by 2） |
| Shift+Z | 视图缩小两倍（Zoom Out by 2） |
| Z | 视图放大 |
| ESC | Cancel |
| Tab | 平移视图（Pan）。按Tab，用鼠标点击视图区中某点，视图就会移至以该点为中心 |
| Delete | 删除 |
| BackSpace | 撤销上一点。很有用，不用因为Path一点画错而删除重画 |
| Enter | 确定一个图形的最后一点，也可双击鼠标左键结束 |
| Ctrl+方向键 | 移动Cell |
| Shift+方向键 | 移动鼠标，每次半个格点的距离 |
| 方向键 | 移动视图 |

快捷键除了Cadence自带的以外，更多的还是自己写的。只要会写Skill程序，就可以自己定义快捷键，其中涉及到对 `.cdsinit` 文件的修改。

联机手册特别提到：不要滥用 Shift+1 这个bindkey。

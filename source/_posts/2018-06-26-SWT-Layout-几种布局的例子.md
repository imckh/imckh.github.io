---
title: SWT Layout 几种布局的例子
date: 2018-06-26 22:22:16
tags: SWT
category: SWT/Jface
---

# SWT中几种布局的例子

## SWT标准布局

|布局     | 作用 |
| ------ | ----- |
|填充布局 | 将控件安排成一行或者一列， 并强制他们大小相同|
|表单布局 | 通过使用`FormAttachments`可选的配置每个子代的上下左右边来定位子代|
|网格布局 | 将子代放置成行和列|
|行布局 | 将子代放置成水平的行或垂直的列|

## FillLayout

### 简介

他把组件摆放在一行或者一列， 并强制组件大小一致。一般组件的高度与最高组件一致，宽度与最宽组件一致。FillLayout不折行，不能设置边界距离和间距。可以使用它布局任务栏或者工具栏，或者Group中的一组选择框。当容器只有一个子组件时也可以使用它，如果一个shell只有一个group子组件，FillLayout将使Group完全充满Shell.

### 示例

> ![SWT.HORIZONTAL](http://oytisdkic.bkt.clouddn.com/filllayouth1.png) <br>
`SWT.HORIZONTAL`水平布局

> ![SWT.HORIZONTAL拉伸](http://oytisdkic.bkt.clouddn.com/filllayouth2.png)<br>
`SWT.HORIZONTAL`水平布局拉伸状态

> ![SWT.VERTICAL](http://oytisdkic.bkt.clouddn.com/filllayoutv1.png)<br>
`SWT.VERTICAL`垂直布局(**default**)

> ![SWT.VERTICAL拉伸](http://oytisdkic.bkt.clouddn.com/filllayoutv2.png)<br>
`SWT.VERTICAL`垂直布局拉伸状态

### 代码
```java
Display display = new Display();
Shell shell = new Shell(display);
FillLayout fillLayout = new FillLayout();
fillLayout.type = SWT.VERTICAL;
// fillLayout.type = SWT.HORIZONTAL;
shell.setLayout(fillLayout);
new Button(shell, SWT.PUSH).setText("B1");
new Button(shell, SWT.PUSH).setText("Wide Button 2");
new Button(shell, SWT.PUSH).setText("Button3");
```
## RowLayout

### 简介

相比`FillLayout`,他可以提供折行显示, 以及可设置的边界距离和间距. 他有几个参数可以通过`setLayoutData`方法设置:
- `type`: 控制水平还是垂直**默认水平**
- `warp`: 控制RowLayout在当前行没有足够空间时是否折行显示组件. **默认折行**
- `pack`:
  - true: 组件使用他们的原始尺寸, 并排列时尽量远离左边. **默认为true**
  - false: 组件将填充可用空间, 跟FillLayout类似
- `justify`:
  -  true: 组件从左至右伸展. 如果容器变大了, 那么多余的空间被平均分配到组件上.
  - 如果`pack`和`justify`同时为true, 组件将保持他们的原始大小, 多余的空间被平均分配到组件之间的空隙上. **默认为false**
- `MarginLeft`, `MarginTop`,`MarginRight`,`MarginBottom` 这些控制组件之间的距离(像素), 以及组件与容器之间的边距, **默认RowLayout保留了3个像素的边距和间距**

### 示例

状态代码 | 初始状态 | 调整尺寸后
--- | --- | ---
wrap = true<br>pack = true<br>justify = false<br>type = SWT.VERTICAL<br> | ![默认](http://oytisdkic.bkt.clouddn.com/wtptjfth1.png) | ![拉伸](http://oytisdkic.bkt.clouddn.com/wtptjfth2.png)
wrap = false<br>没有足够空间时裁边 | ![默认](http://oytisdkic.bkt.clouddn.com/wtptjfth1.png) | ![伸展](http://oytisdkic.bkt.clouddn.com/wfptjfth2.png)
pack = false<br>所有组件尺寸一致 | ![pack = false](http://oytisdkic.bkt.clouddn.com/wtpfjfth1.png) | ![pack = false伸展](http://oytisdkic.bkt.clouddn.com/wtpfjfth2.png)
justify = true<br>组件根据空间进行伸展 | ![justify = true](http://oytisdkic.bkt.clouddn.com/wtptjtth1.png) | ![justify = true伸展](http://oytisdkic.bkt.clouddn.com/wtptjtth2.png)
type = SWT.VERTICAL<br>组件垂直摆放 | ![type = SWT.VERTICAL](http://oytisdkic.bkt.clouddn.com/wtptjftv1.png) | ![type = SWT.VERTICAL调整](http://oytisdkic.bkt.clouddn.com/wtptjftv2.png)

### 代码

共用部分代码
```java
Display display = new Display();
Shell shell = new Shell(display);

// 在这里插入代码

shell.pack();
shell.open();
while (!shell.isDisposed()) {
    if (!display.readAndDispatch()) {
        display.sleep();
    }
}
display.dispose();
```
由于除了layout的配置和按钮添加部分的代码不同外, 其他代码都一样, 所以这里只给出layout配置部分
```java
RowLayout rowLayout = new RowLayout();

rowLayout.wrap = true;
rowLayout.pack = true;
rowLayout.justify = false;
rowLayout.type = SWT.VERTICAL;

rowLayout.marginTop = 5;
rowLayout.marginLeft = 5;
rowLayout.marginRight = 5;
rowLayout.marginBottom = 5;
rowLayout.spacing = 0;
shell.setLayout(rowLayout);

new Button(shell, SWT.PUSH).setText("B1");
new Button(shell, SWT.PUSH).setText("Wide Button 2");
new Button(shell, SWT.PUSH).setText("Button3");
```

### 在RowLayout上配合使用RowData

每个由RowLayout控制的组件可以通过RowData来设置其原始尺寸大小
```java
shell.setLayout(new RowLayout());

Button button1 = new Button(shell, SWT.PUSH);
button1.setText("Button1");
button1.setLayoutData(new RowData(50, 40));
Button button2 = new Button(shell, SWT.PUSH);
button2.setText("Button2");
button2.setLayoutData(new RowData(50, 30));
Button button3 = new Button(shell, SWT.PUSH);
button3.setText("Button3");
button3.setLayoutData(new RowData(50, 20));
```
结果如下所示

![type = SWT.VERTICAL调整](http://oytisdkic.bkt.clouddn.com/rowdataexample.png)

## GridLayout

### 简介

GridLayout是最常用, 功能最强大的标准布局类了, 同时它也最复杂. GridLayout把容器里的组建摆放在一个格子里, 他有许多可设置的参数, 并且同RowLayout类似, 组件可以有相应的布局数据: `GridData`. GridLayout的强大之处就是他可以通过GridData设置每一个控件.

### 示例

#### GridLayout

**常用参数**

- `numColumns`
他是GridLayout最重要的参数, 通常是第一个设置的. 组件从左至右摆放, 当`numColumns + 1`个组件添加到容器中时, 将创建一个新行. 默认值有一列.下面创建5个不同宽度的按钮, 然后展示numColumns为1, 2, 3时的效果.
```java
GridLayout gridLayout = new GridLayout();
gridLayout.numColumns = 1;// 2, 3
shell.setLayout(gridLayout);
new Button(shell, SWT.PUSH).setText("B1");
// 加入五个Button
```

numColumns=1 | numColumns=2 | numColumns=3
--- | --- | ---
![numColumns = 1 ](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_gridLaiout_numcolumns1_2018-07-15_16-16-36.jpg) | ![numColumns = 2 ](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_gridLaiout_numcolumns2_2018-07-15_16-16-36.jpg) | ![numColumns = 3 ](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_gridLaiout_numcolumns3_2018-07-15_16-16-36.jpg)

- `makeColumnsEqualWidth`
强制各列具有相同的宽度, **默认false**

  ![makeColumnsEqualWidth](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_gridLaiout_makeColumnsEqualWidth_numcolumns2_2018-07-15_16-16-36.jpg)
- `MarginWidth, MarginHeight, horizontalSpacing, VerticalSpacing`
与RowLayout类似, 但是不同的是左右边距统一成`marginWidth`, 上下边距统一成`marginHeight`. 同样可以分别设置`horizontalSpacing`和`verticalSpacint`.(RowLayout是根据他的type类型设置水平或者垂直间距)

#### GridData

**构造方式**

`GridData`是`GridLayout`对应的布局数据, 可以通过`setLayoutData`设置组件的布局数据.
可以通过以下几种方式设置`GridData`:
1. 直接设置各参数
```java
GridData gridData = new GridData();
gridData.horizontalAlignment = GridData.FILL;
gridData.grabExcessHorizontalSpace = true;
Button b1 = new Button(shell, SWT.PUSH);
b1.setLayoutData(gridData);
```
2. 通过构造函数
```java
GridData gridData = new GridData(GridData.HORIZONTAL_ALIGN_FILL | GridData.GRAB_HORIZONTAL);
```

**参数**

- `horizontalAlignment, verticalAlignment`
指定组件在格子里按水平或者垂直方式摆放, 可以设置以下值
  - BEGINING (默认)
  - CENTER
  - END
  - FILL

  参考前面五个按钮的例子, 设置三列, 只对button5设置:

alignment | 示例
--- | ---
GridData.BEGINNING | ![BEGINNING](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_griddata_alignment_beginning_2018-07-15_16-16-36.jpg)
GridData.CENTER | ![CENTER](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_griddata_alignment_center_2018-07-15_16-16-36.jpg)
GridData.END | ![END](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_griddata_alignment_end_2018-07-15_16-16-36.jpg)
GridData.FILL | ![FILL](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_griddata_alignment_beginning_fill.jpg)

- `horizontalIndent`
指定组件向右移动指定的像素值, **只有当`horizontalAlignment`为`BEGINNING`时有效, `FIll`时会将左侧不填满**
  - BEGINNING
  ![BEGINNING](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_griddata_indent5_beginning.jpg)
  - FILL
  ![FILL](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_griddata_indent5_fill.jpg)
- `horizontalSpan, verticalSpan`
可以使控件跨越几个格子, 一般与`FILL`一起用
```java
// 将Button5水平扩展两个
gridData.horizontalAlignment = GridData.FILL;
gridData.horizontalSpan = 2;
```
![Span](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_griddata_span_fill_2018-07-15_16-16-36.jpg)
```java
// 将Button2水平扩展两个
gridData.horizontalAlignment = GridData.FILL;
gridData.horizontalSpan = 2;
```
![Span](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_griddata_horspan_fill_button2_2018-07-15_16-16-36.jpg)
```java
// 将Button3垂直扩展两个
// 填充时需要将垂直填充设为true
// gridData.verticalAlignment = GridData.FILL;
// gridData.horizontalAlignment = GridData.FILL;
gridData.verticalSpan = 2;
```
![Span](http://oytisdkic.bkt.clouddn.com/imckh.com/SWT-Layout-%E5%87%A0%E7%A7%8D%E5%B8%83%E5%B1%80%E7%9A%84%E4%BE%8B%E5%AD%90/swtlayout_griddata_verticalspan_fill_button3_2018-07-15_16-16-36.jpg)
- `grabExcessHorizontalSpace, grabExcessVerticalSpace`
当容器增大时, 可以使他们自动增大. 如果设置了横向扩展, 那么用户调整shell宽度时, 组件会自动横向扩展填满新的空间.
**如果有多个用于扩展的组件, 那么他们将平均分配扩展空间**
- `WidthHint, HeightHint`
指示了组件可以具有的宽度和高度, 前提是不与GridLayout其他要求约束矛盾, 一般不会用这个值设置布局


### 代码

```java
Display display = new Display();
Shell shell = new Shell(display);
GridLayout gridLayout = new GridLayout();
gridLayout.numColumns = 3;
shell.setLayout(gridLayout);


GridData gridData = new GridData();
gridData.horizontalAlignment = GridData.FILL;
gridData.verticalAlignment = GridData.FILL;
gridData.verticalSpan = 2;
gridData.grabExcessHorizontalSpace = true;
gridData.grabExcessVerticalSpace = true;

Button b1 = new Button(shell, SWT.PUSH);
// b1.setLayoutData(gridData);
b1.setText("B1");
//
Button b2 = new Button(shell, SWT.PUSH);
// b2.setLayoutData(gridData);
b2.setText("Wide Button 2");

Button b3 = new Button(shell, SWT.PUSH);
b3.setLayoutData(gridData);
b3.setText("Button 3");

Button b4 = new Button(shell, SWT.PUSH);
// b4.setLayoutData(gridData);
b4.setText("B4");

Button b5 = new Button(shell, SWT.PUSH);
//  b5.setLayoutData(gridData);
b5.setText("Button 5");

shell.pack();
shell.open();
while (!shell.isDisposed()) {
    if (display.readAndDispatch()) {
        display.sleep();
    }
}
```
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

## TODO 2018-06-27 22:28:35 GridLayout

### 简介

### 示例

### 代码
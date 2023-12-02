# Java绘图功能

![image-20231130143313003](https://raw.githubusercontent.com/tingzhuk/myNote/master/java%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/java%E5%9F%BA%E7%A1%80/image/image-20231130143313003.png)

在 Java  的绘图坐标系是这样的  原点是左上角

> Component 类提供了两个和绘图相关最终要的方法

1） `paint(Graphics g)` 绘制组件的外观

2）`repaint()` 刷新组件的外观



> 当组件第一次在屏幕上显示的时候，程序会自动调用 paint()   方法来绘制组件
>
> 以下情况 paint（） 将会被调用

1）窗口最大化、窗口最小化

2）窗口大小发生变化的时候

3）repaint  方法被调用的时候



![image-20231130152134783](https://raw.githubusercontent.com/tingzhuk/myNote/master/java%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/java%E5%9F%BA%E7%A1%80/image/image-20231130152134783.png)



* 案例 1  绘制圆形

```java
package com.wxledu.draw;

import javax.swing.*;
import java.awt.*;

/**
 * @author xxx
 * @version 1.0
 * 演示如何在面板上画圆形
 */
public class DrawCircle extends JFrame{  //JFrame  对应一个窗口， 可以理解为 一个画框

    //定义一个面板
    private MyPanel mp = null;
    public static void main(String[] args) {
        new DrawCircle();
    }

    public DrawCircle(){ //构造器
        //初始化面板
        mp = new MyPanel();
        //把面板放入窗口（画框）
        this.add(mp);
        //设置窗口大小
        this.setSize(400, 300);
        // 点击窗口的 小 ×  ， 程序就完全退出
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setVisible(true); //可以看到
    }
}

// 1. 先定义 一个面板 MyPanel,  继承 JPanel类， 后面我们画图形就在这个面板上画
class MyPanel extends JPanel{

    // MyPanel  理解为画板
    // Graphics g   把 g  理解为 画笔
    // Graphics   提供了很多画图的方法
    @Override
    public void paint(Graphics g) {//绘图方法
        super.paint(g); //调用父类的方法完成初始化
        System.out.println("paint方法被调用");
        g.drawOval(5, 5,100,100);
    }
}

```

## Graphics 类

Graphics 类 可以理解为**画笔**， 提供各种绘制图形的方法

* 画直线 `drawLine( int x1, int y1, int x2, int y2)`

* 画矩形边框 `drawRect( int x, int y, int width, int height)`

* 画椭圆边框 `drawOval(int x, int y, int width, int height)`

* 填充矩形 `fillRect(int x, int y, int width, int height)`

* 填充椭圆 `fillOval(int x, int y, int width, int height)`

* 画图片 `drawImage(Image img, int x, int y, ....)`

  ```java
  // 1. 获取图片资源  /bg.png  表示在该项目的根目录去 获取 bg.png  图片资源
          Image image = Toolkit.getDefaultToolkit().getImage(Panel.class.getResource("/bg.png"));
  ```

  

* 画字符串 `drawSring(String str, int x, int y)`

  ```java
  // 设置	颜色
  g.setColor(Color.red);
  // 设置字体
  g.setFont(new Font("隶书",Font.BOLD,50));
  // 设置的 x y  是北京你好的  左下角
  g.drawString("北京你好",100,100);
  ```

  

* 设置画笔字体 `setFont(Font font)`

* 设置画笔颜色 `setColor(Color c)`
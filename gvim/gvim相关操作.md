`c + i + "` 重写双引号里面的所有东西

`c + i + w` 重写这个单词

`y + -> 5` 向右复制五个字符

`p` 粘贴

`f + 字母` 光标跳到下个字母的位置

`0` 零 回到行首最开头

```
"i like you: gbuyigyu"
```

假如当前在行首, 删掉冒号之前的东西

`d + f + :`  删除直到找到空格

`c + f + :` 删除冒号之前的所有东西并且进入写入模式

`y + f + :` 复制冒号之前的所有内容

`/要查找的内容`  查找

---------------------------------------------------------------------

`A` 行尾

`a` 行首

`G` 最后一行

``v` 进入可视模式   是普通可视模式

`Shift + v` 是行可视模式， 可以直接选中这一行，按上下键选中多行

选中之后按 冒号进入 normal 指令模式![image-20231201212758349](https://boimage.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20231201212758349.png)

`:'<,'>normal A,png` 在选中的这些行的行尾都加   .png

`:'<,'>normal Kmy allpaper` 在每一行的开始加上 my wallpaper，   （这里的 K  可能值  I）

![image-20231201213256036](https://boimage.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20231201213256036.png)

`d`  是剪切操作

`c`  是更改

`v + 上下左右键`  普通模式下 按 v + 上下左右键可以选中   按一下 `d`  可以把这一段进行剪切 

-------------------------------

![image-20231201215809051](https://boimage.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20231201215809051.png)

`Ctrl + v` 进入可视块模式 G 全选到底部， 把每一行的额第一个字母都选中，然后可以移动光标增加选中

------------------------------

![image-20231201220038644](https://boimage.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20231201220038644.png)

把首字母小写的  w  改成大写的 W ， 在第一行的第一个字母`Ctrl + v` 进入可视块，然后 G 到最后一行，d 删除每一行的第一个字母， 然后在进入  可视块模式 `Ctrl + v` 从第一个选到最后一个，按`Shift + i` ,  然后 （这时候光标会只在第一行，不要慌张）打一个 大写的 W，然后按 `Esc` 会发现，所有选中的地方都添加了 大写字母  W



------------------------------------------

分屏
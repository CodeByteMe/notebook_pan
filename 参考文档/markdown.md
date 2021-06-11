# 标题类
```markdown
# 第一级标题 
```
# 第一级标题 <!-- {docsify-ignore} -->

```markdown
文字下边放一行等号也可以进行一级标题的显示
==================================== 
```
# 文字下边放一行等号也可以进行一级标题的显示 <!-- {docsify-ignore} -->

```markdown
## 第二级标题
```
## 第二级标题

```markdown
文字下边放一行中划线也可以进行二级标题的显示
--------------------------------------
```
## 文字下边放一行中划线也可以进行二级标题的显示

```markdown
### 第三级标题
```
### 第三级标题 

```markdown
#### 第四级标题
```
#### 第四级标题

```markdown
##### 第五级标题
```
##### 第五级标题

```markdown
###### 第六级标题（第六级标题就是最小的标题了）
```
###### 第六级标题（第六级标题就是最小的标题了）

-----------------------------------------------------
# 正文
```markdown
正文直接输出就可以
如果直接换行，那么相当于空了一个格，并没有真正换行  
需要每行后边加两个空格来进行换行

如果多空一行相当于多一些间距，也就开始了下一段
```  
正文直接输出就可以
如果直接换行，那么相当于空了一个格，并没有真正换行  
需要每行后边加两个空格来进行换行

如果多空一行相当于多一些间距，也就开始了下一段

# 分割线
```markdown
用三个以上的星号可以创建分割线
***************************
```
***************************
```markdown
同时可以使用三个以上的下划线来创建分割线
___________________________
```
___________________________

# 代码块的创建
````markdown
```java
/**
 * 这个是用来进行代码块封装的.
 * 第一个```后边加上语言就可以进行代码高亮了。
 */
import java.util.HashMap;

public class Test {
    public static void main(String[] args) {
        System.out.println("我是用markdown写出来的");
    }
}
```
````

```java
/**
 * 这个是用来进行代码块封装的.
 * 第一个```后边加上语言就可以进行代码高亮了。
 */
import java.util.HashMap;

public class Test {
    public static void main(String[] args) {
        System.out.println("我是用markdown写出来的");
    }
}
```

如果想要直接输出代码块，也可以缩进四个空格  

    // 这样不能指定语言进行高亮
    import java.util.HashMap;
    public class Test {
        public static void main(String[] args) {
            System.out.println("我是用markdown写出来的");
        }
    }
!> 小技巧
若想要在markdown的格式中嵌套markdown代码, 可以如下  
`````markdown
每外边多加一层, 就多加一个反引号`即可
````markdown
```java
/**
 * 这个是用来进行代码块封装的.
 * 第一个```后边加上语言就可以进行代码高亮了。
 */
import java.util.HashMap;

public class Test {
    public static void main(String[] args) {
        System.out.println("我是用markdown写出来的");
    }
}
```
````
`````
```markdown
正文中的文字高亮可以用一个反引号高亮`show1()`, 也可以用两个反引号高亮``show2()``
```
正文中的文字高亮可以用一个反引号高亮`show1()`, 也可以用两个反引号高亮``show2()``

# 区块的创建
```markdown
> 大于号可以创建一个区块
> > 再多加一个大于号可以作为子区块
> > 1. 当然区块中也可以嵌套列表
> >    1. 在列表的作用下，区块可以显示的更丰富
```
> 大于号可以创建一个区块
> > 再多加一个大于号可以作为子区块
> > 1. 当然区块中也可以嵌套列表
> >    1. 在列表的作用下，区块可以显示的更丰富

# 有序列表
```markdown
1. 数字加.加空格就可以做一个有序列表
2. 我是有序列表的第二个
   1. 当进行了有序列表的缩进后，
      1. 自动生成了多级列表
```
1. 数字加.加空格就可以做一个有序列表
2. 我是有序列表的第二个
   1. 当进行了有序列表的缩进后，
      1. 自动生成了多级列表

# 无序列表
```markdown
- 前边加一个短中横线就是无序列表
  - 当无序列表进行了缩进，那么它就可以产生多级列表
  + 也可以使用加号
  * 或者星号也可以作为无序列表
* 列表中同样
* > 可以加入区块来丰富样式
```
- 前边加一个短中横线就是无序列表
  - 当无序列表进行了缩进，那么它就可以产生多级列表
  + 也可以使用加号
  * 或者星号也可以作为无序列表
* 列表中同样
* > 可以加入区块来丰富样式

# 文字样式的改变
## 加粗
```markdown
如果想要让文字**加粗**，那么使用两个*就可以实现了
```
如果想要让文字**加粗**，那么使用两个*就可以实现了

## 倾斜
```markdown
如果想要文字*倾斜*，使用一个*来包裹文字
```
如果想要文字*倾斜*，使用一个*来包裹文字

## 加粗并倾斜
```markdown
如果既想要***文字加粗，又想要文字倾斜***，三个*可以搞定
```
如果既想要***文字加粗，又想要文字倾斜***，三个*可以搞定

## 删除线
```markdown
~~删除线~~需要加两个~来包裹文字
```
~~删除线~~需要加两个~来包裹文字

## 下划线
```markdown
下划线可以使用<u>HTML标签</u>来实现
```
下划线可以使用<u>HTML标签</u>来实现

# 链接
## 普通样式
```markdown
<https://www.baidu.com>  
[知乎专栏](https://zhuanlan.zhihu.com/p/56943330)  
[菜鸟教程](https://www.runoob.com/markdown/md-title.html)
```
<https://www.baidu.com>  
[知乎专栏](https://zhuanlan.zhihu.com/p/56943330)  
[菜鸟教程](https://www.runoob.com/markdown/md-title.html)  

## 高级样式
为网址进行变量赋值,可以统一进行链接添加  
```markdown
这里使用`1`作为网址变量[百度][1]  
这里使用`文字`作为网址变量[搜狗][文字]  

[1]: https://www.baidu.com
[文字]: https://www.sogou.com
```
这里使用`1`作为网址变量[百度][1]  
这里使用`文字`作为网址变量[搜狗][文字]  

[1]: https://www.baidu.com
[文字]: https://www.sogou.com

# 添加图片
```markdown
![alt 我是一张图片](https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png)  
![alt 我是一张图片](https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png "这里再加一个鼠标悬浮提示")  
```
![alt 我是一张图片](https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png)  
![alt 我是一张图片](https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png "这里再加一个鼠标悬浮提示")  
这里可以使用img标签来修改图片尺寸  
```markdown
<img src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png" width="50%">
```
<img src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png" width="50%">

# 表格
使用`|`来分割表格,使用`-`来分割表头与其他行  
`-:`可以设置右对齐
`:-`可以设置左对齐
`:-:`可以设置居中对齐
```markdown
| score | name | age |
| :---  | :--: | ---:|
|  12   | lee  | 56  |
|  13   | tank | 22  |
|  4    | easul| 25  |
```

| score | name | age |
| :---  | :--: | ---:|
|  12   | lee  | 56  |
|  13   | tank | 22  |
|  4    | easul| 25  |

# 高级技巧
可以在<kbd>markdown</kbd>里边直接使用`div`标签来进行html页面操作  
[高级技巧](https://www.runoob.com/markdown/md-advance.html)可以点击参考

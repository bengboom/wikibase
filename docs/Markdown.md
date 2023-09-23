# markdown学习

- [markdown学习](#markdown学习)
  - [标题](#标题)
  - [字体](#字体)
  - [分割线](#分割线)
  - [列表](#列表)
  - [链接](#链接)
  - [图片](#图片)
  - [引用](#引用)
  - [代码](#代码)
    - [单行](#单行)
    - [多行](#多行)
  - [表格](#表格)
  - [表格对齐方式](#表格对齐方式)
  - [HTML嵌套](#html嵌套)
  - [转义](#转义)
  - [公式 LaTeX](#公式-latex)

## 标题

```markdown
# 大标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

## 字体

```markdown
*斜体*
**粗体**
***斜粗体***
~~划去~~
<u> Undeilined</u>
```

## 分割线

```markdown
***
* * * * * * *        
*****
-------
-- -- --
-- -- --
```

## 列表

```markdown
- 第一项
- 第二项

1. 第一项
2. 第二项
```

- 第一项
- 第二项
  
1. 第一项
2. 第二项

## 链接

```markdown
<https://blog.bengboom.ltd>

[bengboom的blog](blog,bengboom.ltd)
```

<https://blog.bengboom.ltd>

[bengboom的blog](blog,bengboom.ltd)

## 图片

```markdown
![name](address)
```

## 引用

```markdown
> 引用内容
```

> 引用内容

## 代码

### 单行

```markdown
`cout<<"Hello World"<<endl;`
```

`cout<<"Hello World"<<endl;`

### 多行

```markdown
```cpp
#include<iostream>
using namespace std;
int main(){
    for(int i=1;i<=n;i++)
};
void()
double bool int float char struct vector stl stack queue
```

```cpp
#include<iostream>
using namespace std;
int main(){
    for(int i=1;i<=n;i++)
};
void()
double bool int float char struct vector stl stack queue
```

## 表格

```markdown

| 1    | 2    | 3    |      |      |
| ---- | ---- | ---- | ---- | ---- |
|      |      | 6    |      |      |
|      |      | 9    |      |      |
|      |      | 12   |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |

```

| 1    | 2    | 3    |      |      |
| ---- | ---- | ---- | ---- | ---- |
|      |      | 6    |      |      |
|      |      | 9    |      |      |
|      |      | 12   |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |

## 表格对齐方式

：-左对齐

：-：居中对齐

## HTML嵌套

```markdown
<kbd> keyboard</kbd>
<b>hey</b>
<i>?</i>
<em>？</em>
<sup>????</sup>123
<sub>?????</sub>123
<br>
```

## 转义

Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：

```tex
\   反斜线
`   反引号
*   星号
_   下划线
{}  花括号
[]  方括号
()  小括号
#   井字号
+   加号
-   减号
.   英文句点
!   感叹号
```

## 公式 LaTeX

>Markdown Preview Enhanced 使用 KaTeX 或者 MathJax 来渲染数学表达式。
>
>KaTeX 拥有比 MathJax 更快的性能，但是它却少了很多 MathJax 拥有的特性。
>你可以查看 KaTeX supported functions/symbols 来了解 KaTeX 支持那些符号和函数。
>
>--[菜鸟教程](https://www.runoob.com/markdown/md-advance.html)

```markdown
$$det(A)=0$$

$$
Ax=b
$$
```

$$det(A)=0$$

$$
Ax=b
$$

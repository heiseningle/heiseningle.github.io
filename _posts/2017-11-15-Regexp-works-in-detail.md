---
layout:     post
title:      Regexp Acts in Detail 
subtitle:   正则表达式的工作原理和应用实例
date:       2017-11-15
author:     zz
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - regexp
---



## Preface
- 从2015年到2017年，遇到不少需要处理文本数据的情景：ad的文本处理、销售部门拼接、文本脏数据替换、公司实体资质替换等。
同时，应用的环境也不尽相同：在DB里、在R、甚至在excel里面。
因此，整理了如下的bullet point和references供查阅

## 正则引擎分类
- 正则引擎分成两类，
    - deterministic finite automation (DFA)
    - non- deterministic finite automation (NFA)
        - POSIX NFA
        - tradition NFA
一般用的都是tradition NFA，区别：是否提供“回溯”机制

## 正则匹配的关键概念：pattern、string、匹配机制
1. pattern，匹配文本
    - 控制权
    - 通配、数量修饰、优先修饰
    - 特殊字符
    - 位置匹配（零宽度匹配）

2. string，被匹配文本
    - 位置:字符和字符间被称作位置 
      - 比如，“ABC”，共有三个字符、四个位置{0,1,2,3}
      - 匹配机制是以位置为步长前进的

3. 匹配机制
- 匹配过程&匹配结果
    - 匹配过程
        - 按轮数逐个位置前进，pattern进行对string的捕捉，每向前推进一个位置称为一轮
        - 在一轮当中，每次在贪婪/非贪婪的选择都会产生“回溯状态”，以备回溯。类似二叉树、“平行宇宙”
        - 位置匹配（零宽度）是非独占的，即多个pattern都可以匹配同一一个位置；而非位置匹配是独占的，即一个pattern子项仅能匹配一个内容，当匹配不上只能出让控制权
    - 匹配结果
        - 当轮数前进到string的结束位置，将输出匹配结果
        - 若成功，则输出内容/分组内容；若失败，报告失败
- 位置匹配&非位置匹配
    - **非位置匹配**会产生匹配内容，并且会保存，并在体现在匹配结果上
    - **位置匹配（零宽度匹配）**仅影响过程，不会产生匹配内容，也不会在体现在结果当中


```
通配时才存在贪婪与否
    - 例如，string = c('<a><b>', '<a><>', '<><>', ' ', NA); pattern = "<(.*?)>” 
    - ？修饰了quantifier“ * ”，*?共同修饰了通配符“ . ”，因此结果会出来<a> 而不是直接<a><b>。因为限制了通配符的贪婪 
    - 但是在非通配匹配时，不存在贪婪一说。只要能匹配的上的就会被匹配出来。
```

## 正则pattern符号分类

1. meta character 
. \  | ：预定义类
^ $ \b \B：锚点
{ } * + ?：控制次数
[ ]: 字符类 、 范围类
( )  ?:  ：分组
? : greedy / non-greedy
    

2. literal character 

3. pre-defined:
blank 
换行
回车
分页
制表符 (tab)
垂直制表


4. 回溯机制
look ahead ?>pattern,means that right hand side of question mark would be used as matched pattern without tracing back  

look backward


## Refs
- 入门读物：http://gold.xitu.io/post/582dfcfda22b9d006b726d11

- 原理：http://blog.csdn.net/lxcnn/article/details/4304651

- 经典读物：http://deerchao.net/tutorials/regex/regex.htm

- 进阶读物：https://segmentfault.com/a/1190000000426455

- interactive tutorial: https://regex101.com/

- 图像化正则：https://regexper.com/

- R正则：https://www.regular-expressions.info/rlanguage.html

- 测试正则是否通过：https://regexr.com/














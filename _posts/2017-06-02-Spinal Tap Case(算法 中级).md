---
author: Demi_YuHongJun
comments: true
date: 2017-06-02 13:42:32+00:00
layout: post
title: Spinal Tap Case(算法 中级)
description: Spinal Tap Case(算法 中级)
keywords: FreeCodeCamp
categories:
- Tech
tags:
- FreeCodeCamp
---
* 目录
{:toc}
---

https://www.freecodecamp.cn/challenges/spinal-tap-case

将字符串转换为 spinal case。Spinal case 是 all-lowercase-words-joined-by-dashes 这种形式的，也就是以连字符连接所有小写单词。

如果你被卡住了，记得开大招 Read-Search-Ask。尝试与他人结伴编程、编写你自己的代码。

这是一些对你有帮助的资源:

[RegExp](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

[String.replace()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)

>spinalCase("This Is Spinal Tap") 应该返回 "this-is-spinal-tap"。

>spinalCase("thisIsSpinalTap") 应该返回 "this-is-spinal-tap"。

>spinalCase("The_Andy_Griffith_Show") 应该返回 "the-andy-griffith-show"。

>spinalCase("Teletubbies say Eh-oh") 应该返回 "teletubbies-say-eh-oh"。

##### 思路一

整体思路就是用正则表达式替换字符串指定字符。难点在于正则的写法。现逐步分析。

观察第三个测试语句，发现有的单词之间用下划线 "_" 连接，其它用空格连接，需要把它们转换成同一种占位类型，方便最后统一转换成连接符 "-" 。这里先把下划线转为空格。

str = str.replace(/_/g," ")
测试 spinalCase("The_Andy_Griffith_Show") ，结果为：

The Andy Griffith Show
观察第二个测试语句，单词之间并无空格或其他连接符，而是以首字母大写作为单个单词的判断标准。在大写字母之前添加空格，与上面代码保持一致。本解法中，所有单词分隔在转为 "-" 之前都用空格表示。

str = str.replace(/([A-Z])/g," $1");
其中，小括号表示分组， $1 表示第1个小括号捕获内容。

测试 spinalCase("thisIsSpinalTap") ，结果为：

this Is Spinal Tap
然而，这样做之后，有的语句因为首单词的首字符也是大写的，所以前面也会多个空格。我们需要排除这种情况：

str = str.replace(/^\s/,"");
其中 "^" 表示语句开头， "\s" 表示空白符。

接下来，可以把所有空白符替换为题目要求的连接符 "-" ：

str = str.replace(/\s+/g,"-");
其中， "+" 表示匹配前一项一或多次，如果不加这个，有一个以上空格的地方会同时出现多个 "-" 并用情况。

最后，把所有字符转为小写，完成！
```javascript
function spinalCase(str) {
  // "It's such a fine line between stupid, and clever."
  // --David St. Hubbins
  str=str.replace(/_/g,' ')
         .replace(/([A-Z])/g,' $1')
         .replace(/^\s/g,'')
         .replace(/\s+/g,'-')
         .toLowerCase();
  return str;
}

spinalCase('This Is Spinal Tap');

```
##### 思路二：
看测试用例，需要转换的字符串格式可以归为两类。第一类是利用空格、下划线等符号分解命名、还有第二类是在座诸位比较熟悉的驼峰命名法。以下称符号类型或驼峰类型。

符号类型转换形式是简单的，驼峰类型转换形式也是简单的，但这二者并不相同，怎么区分这二者就是一个难点了。我们可以先定一个可以达到的小目标，比如先把这两种字符串类型分开。博主的方法是将字符串的按照符号类型分解，如果分解后的数组长度为1也就说明了当前字符串不管正不正经，他都是一个驼峰类型的字符串。

 str.split(/\W|_/).length==1 

既然区分开两种字符串类型了，那就把两种字符串类型都转换成题目要求的那样：我-是-吴-彦-祖 这种类型。

驼峰类型转换很简单，只需要历遍字符串，使用正则表达式判断字符串中的某个字符是否为大写，如果是大写转换成小写再在前面加一根短短的东西。方法如下：


解法二：
```javascript
function spinalCase(str) {
  // "It's such a fine line between stupid, and clever."
  // --David St. Hubbins
     if(str.split(/\W|_/).length==1){
           console.log(str);
      for(var i=0;i<str.length;i++){
    
        if(/[A-Z]/.test(str[i])){
          if(i===0){
               str=str.replace(str[i],str[i].toLowerCase());

          }else{
               str=str.replace(str[i],"-"+str[i].toLowerCase());
          }
        }
      }
    }else str=str.toLowerCase().split(/\W|_/).join("-");
    return str;
}

spinalCase('ThisIsSpinalTap');

```

---
layout: post
title: "shell通配符和正则表达式中的常用符号"
subtitle: 'wildcard character and regular expression'
author: "cslqm"
header-style: text
tags:
  - Linux
---

通配符是常用于linux shell中，而正则表达式使用范围更加广泛，一些儿shell命令也支持，比如sed、grep，awk等。
总是搞混通配符和正则表达式中的符号，毕竟他们都是同一符号只是功能不一样而已。

常见通配符及其表达意思如下：

| 通配符 | 作用 |
| --- | --- |
| * | 匹配任意长度的任意字符 |
| ? | 匹配任意单个字符 |
| [] | 匹配指定范围内的任意单个字符 |
| [^] | 匹配指定范围之外的任意单个字符 |
| [:space:] | 空白字符 |
| [:punct:] | 标点符号 |
| [:lower:] | 小写字母 |
| [:upper:] | 大写字母 |
| [:alpha:] | 大小写字母 |
| [:digit:] | 数字 |
| [:alnum:] | 数字和大小写字母 |

正则表达中这些常用的符号及其意义。

| 正则表达式的符号 | 作用 |
| --- | --- |
| * | \*前的子表达式出现零次或多次。比如go\*gle就可以匹配到google |
| ? | 匹配前面的子表达式零次或一次（方便记忆可以联想c中的'? :'）。 |
| [] | 字符集合，匹配集合中的任一字符 |
| [^] | 匹配任何不在指定范围内的任意字符，'\[^a-z\]' 可以匹配任何不在 'a' 到 'z' 范围内的任意字符 |
| + | 匹配前面的子表达式出现一次或者多次 |

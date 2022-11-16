---
layout  : wiki 
title   : Inversion of control
summary :
date    : 2018-01-20 17:43:19 +0900
updated : 2019-08-13 13:50:57 +0900
tag     : tdd
toc     : true
public  : true
parent  : [[/tdd/martin-fowler]]
latex   : true
---
* TOC
{:toc}

## 제어의 역전 (Inversion Of Control)
> 이 글은 마틴 파울러 블로그의 [Inversion Of Control](https://martinfowler.com/bliki/InversionOfControl.html)을 번역한 글입니다.

```
Inversion of Control is a common phenomenon that you come across when extending frameworks. Indeed it's often seen as a defining characteristic of a framework.
```
* 
```
Let's consider a simple example. Imagine I'm writing a program to get some information from a user and I'm using a command line enquiry. I might do it something like this
    #ruby
    puts 'What is your name?'
    name = gets
    process_name(name)
    puts 'What is your quest?'
    quest = gets
    process_quest(quest)
In this interaction, my code is in control: it decides when to ask questions, when to read responses, and when to process those results.
```
* 
---
layout: post
title: pig Latin高级应用
tag: pig
---

### foreach的高级功能之一
#### flatten
有时用户的数据是存放在bag或者tuple中的，而用户想降低嵌套的级别。
举个栗子：<br/>
因为一个运动员可以充当多种角色，因此position(角色)这个字段是以bag类型存储的。<br/>
这样可以允许我们在baseball这个文件中仍然保证一个运动员对应一个条目。但是当用户想快速地浏览数据然后根据一个特定的角色进行分组的时候，用户需要一种将这些条目从bag中分离出去。
为了完成这个需求，Pig在foreach中提供了flatten修饰符:

    --flatten.pig
    players=load 'baseball' as (name:chararray,team:chararray,position:bag{t:(p:chararray)},bat:map[]);
    pos=foreach players generate name,flatten(position) as position;
    bypos=group pos by position;

包含一个flatten的foreach会导致bag中的每条记录会和其它所有generate语句中的表达式进行交叉乘积。我们来看下baseball文件中的第一条记录

    Jorge Posada,New York Yankees,{(Catcher),(Designated_hitter)},...
一旦这行记录经过flatten语句处理后，将会产生两天记录：

    Jorge Posada，Catcher
    Jorge Posada，Designated_hitter
    
    

> 如果有多个bag，而且每个都经常做flatten处理，那么这交叉乘积运算将使得bag的成员和generate语法中的其他表达式一样显示。因此结果将不是n行（这里的n表示一个bag中的记录数），而将是n*m行。

有一个可能会使很多用户惊讶的不好的方面是如果这个bag是空的，那么将不会产生任何记录。因此如果baseball文件中有一个条目是没有位置的，不管是因为bag是null还是空的，那么这条记录就不会包含在脚本flatten.pig的输出结果中。包含空bag的记录将会被foreach语句抛弃。
采用这种处理方式的原因有两个。

 1. 既然Pig可以知道也可以不知道bag中数据的模式，那么它也就可能不知道怎么去为确定的字段填补什么样的值。
 2. 从数据角度来看，这也可能正是用户所期望的。将集合S和一个空集合做交叉乘积，结果是一个空集合。如果用户想避免这种情况，那么可以使用一个三元条件表达式将空bag替换成一个常量bag:


    --flatten_noempty.pig
    players=load 'baseball' as (name:chararray,team:chararray,position:bag{t:(p:chararray)},bat:map[]);<br/>
    noempty=foreach players generate name,flatten(position is null or IsEmpty(position)?{('unknown')}:position) as position;<br/>
    pos=foreach noempty generate name,flatten(position) as position;<br/>
    bypos=group pos by position;<br/>
	
	

> 对于tuple也同样可以使用flatten语句。在这种情况下，它不会产生一个交叉乘积结果，而它会使tuple中的每个字段转换成顶层的字段，对于空tuple将会移除整条记录。
> 
> 如果bag或者tuple中的字段在进行flatten处理的时候命名了别名，那么pig会一并将这些别名向数据流下个节点传。在进行join操作是，为了避免模糊不清，字段名将加上bag的名称并且中间使用::符号连接。只要字段名不是模糊不清的，那么用户也并非一定要使用bag名称::这个前缀。
> 
> 如果用户想改变这些字段的别名，或者这些字段在初始化的时候并没有一别名，那么可以在flatten语句后加上as语句来指定字段别名，正如前面展示的例子中所使用的方式。如果bag或者tuple中有超过一个字段需要指定别名，那么用户需要使用圆括号将这组字段别名括起来。

> 最后需要说明的是，如果用户对没有模式的bag或者tuple进行flatten操作，同时没有使用as语句指定字段名时，那么这个foreach语句的输出结果也将是没有模式的。这是因为Pig并不知道flatten语句将会在结果中产生多少个字段。

来源《Pig编程指南》【美】Alan Gates著
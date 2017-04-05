---
title: 前端面试题
date: 2017-02-13 15:10
tags:
  - 前端开发
  - 前端面试题
  - HTML5简介
categories: HTML5学习笔记
---



+  使用正则表达式精确匹配出如下字符串中加粗部分的 "3052202501"。ptui_loginuin=3052202501%20;pt2gguin=o3052202501;uin=o**3052202501**;ptisp=ctc
   
        如果配合其他的字符的话,可以知道,加粗的那个"3052202501"
        var rg1 = /uin=o(\d{10})/g;
        var match = rg1.exec(str);
        var index = parseInt(match.index)+5
        console.log("匹配的字符串的位置在:"+index);
        console.log("匹配到的字符串为:"+match[1]);
        
 +  完成方法count(str)，传入字符串，返回字符串中出现次数最多的字符及次数
    
        function count(str){
                var length = str.length;
                if(length ==0){
                    return;
                }
                var max = 1;
                var maxChar = str[0];
                var obj = {};
                for(var i = 0;i<length;i++){
                    if(str[i] in obj){
                        obj[str[i]] ++;
                        if(max < obj[str[i]]){
                            max = obj[str[i]];
                            maxChar = str[i];
                        }
                    }else{
                        obj[str[i]] = 1;
                    }
                }
               return {
                   maxNum : max,
                   maxChar :maxChar
               }
            }
 
 
 + 假设你有一个函数rand5()，产生[0, 5)之间的随机整数，每个数字概率1/5，如何使用这个函数产生[0, 7)之间的随机整数，每个数字概率1/7
    
    + 正确的方法是利用rand5()函数生成1-25之间的数字，然后将其中的1-21映射成1-7，丢弃22-25。

    
  
 
 + 完成方法subset(arr1,arr2)，传入两个数组，判断数组arr1是否为数组arr2的子集
 
 + 现在有一个基准数组 records，先要求你维护其子集 selection 数组（初始为空），维护操作包含删除和插入。 插入：给定 records 数组中的一个元素，插入到 selection 中。删除：给定 records 数组中的一个元素，把它从 selection 中删除。现在要求你：实现 insert() 和 remove() 方法来实现以上操作。设计一个算法，保证每次维护操作后，保持 selection 数组中的元素的偏序关系与 records 数组中的保持一致，分析你算法的复杂度。
 

 
 



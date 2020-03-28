---
title: leetCode 无重复字符的最长子串
author: huicanvie
layout: post 
---

```
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-substring-without-repeating-characters

```
```
题解：
思路1：比较暴力的方式，首先就想到先找出来这个最长子串，然后计算出其长度。首先将字符串转为数组，循环数组。
      从第一个数组元素开始，判断临时数组里是否已经存在当前字符，如果不存在，则将当前字符存入临时数组，
      如果已经存在了，则表明找到了一个没有重复数组的子串，
      将此中间临时数组存入结果集数组中,清空中间临时数组，再从第二个数组元素开始，重复之前的过程。。。
思路2：仔细看题，只要求是返回其长度，所以在思路1的基础上进行了优化。直接循环读取字符串中的字符，
      设置一个临时字符串，用于存放在循环过程中的得到的子串
      当临时字符串中没有当前字符，则把该字符加到临时字符串中，
      同时最关键的一点是，将max和当前临时字符串的长度做比较，大于max的话，则将此长度设置为max的值。
      这样，循环结束后，max就是我们想要的结果。
总结： 在leetcode上运行后，思路1运行耗时比思路2多出将近200ms！
      很明显，思路2比思路1的方法效率高，代码也简洁许多。
```
> 思路 1

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
     var srcArr = s.split('')
     var tempArr = []   // 中间临时数组
     var resArr = []  // 结果集数组
     var index = 0
     var max = 0
     var len = srcArr.length
     var stop = false
    while (index < len) {
        if (!stop) {
           for(var i = index; i < len; i++) {
                if (tempArr.indexOf(srcArr[i]) < 0) {
                        tempArr.push(srcArr[i])
                        // console.log('tempArr,,,',tempArr)
                        if (i == len - 1) {
                            resArr.push(tempArr.join(''))
                            tempArr = []
                            stop = true
                            break
                        }
                } else {
                    
                    resArr.push(tempArr.join(''))
                    tempArr = []
                    break
                }
           }
        }
        index++  
    }
    // console.log('resArr,,,',resArr)
     resArr.forEach(item => {
         if (item.length > max) {
             max = item.length
         }
     })
     
     console.log(max)

     return max
};
```

> 思路 2

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    let max = 0
    let count = 0
    while (count < s.length) {
        let str = ''
        // console.log('str,,,',str)
        for (let i = count,len = s.length; i < len ; i++) {
            let char = s.charAt(i)
            if (str.indexOf(char,0) < 0) {
                str += char
                max = max > str.length ? max : str.length  // 关键点
            } else {
                break
            }
        }
        count++
    }
    
    return max
};
```
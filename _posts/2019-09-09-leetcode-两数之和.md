---
title: leetCode 两数之和
author: huicanvie
layout: post 
---

```
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，
并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/two-sum

```
```
题解：
思路1：首先想到是2层循环,外循环确定左边的数，内部循环剩余的数和其外循环的数相加，
      等于目标值就返回结果
思路2：我们还是要做一次循环，在内循环中，我们考虑利用数组来存储当前数和目标值的差值
      临时数组中存放已经比对过的数，利用indexOf的方法，如果返回的是>-1，则表明找到了此差值，
      然后再原数组中去查询此差值的index，最后返回
```
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */

var twoSum = function(nums, target) {
    for(var i = 0,len = nums.length; i < len; i++) {
         for(var j = i + 1,len = nums.length; j <len;j++) {
             if (nums[j] + nums[i] === target) {
                 return [i,j]
             }
         }
    }
};
```
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */

var twoSum = function(nums, target) {
    let temp = []
    for(var i = 0,len = nums.length; i < len; i++) {
        let diff = target - nums[i]
        if (temp.indexOf(diff) > -1) {
            let y = nums.indexOf(diff)
            return [i,y]
        }
        temp.push(nums[i])
    }
};
```
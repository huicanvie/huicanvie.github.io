---
title: leetCode 寻找两个有序数组的中位数
author: huicanvie
layout: post 
---

```
给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

示例 1:

nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
示例 2:

nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/median-of-two-sorted-arrays

```
```
题解：
思路1：首先合并2个数组，从示例里可以得出，要分数组长度的奇偶情况。
      奇数总长度，则是下标为(length+1)/2 -1 的元素，
      如果是偶数，则应该是length / 2 -1 和 length /2 两个下标的元素，相加再除以2.0
思路2：二分法，假设合并数组后的中位数下标是n，可以推断分别在2个原数组中，肯定存在 i+ j = n，
      假设已经找到这个数，这个数在nums1的位置i，在此数前有i个元素，同理，在nums2的位置j，j下标前有j个元素
      合并后，比中位数小的元素有（i+j）个，中位数在n位，下标从零开始计算，则在n位，那么在n 之前就有n个元素
总结：虽然2个数组都是有序的，但是合并操作，时间复杂度是O(m+n),不满足O(log(m + n))，应该使用二分法，后续会把二分法加上
```
> 思路 1

```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */
var findMedianSortedArrays = function(nums1, nums2) {
    let arr = nums1.concat(nums2)
    arr = arr.sort((a,b) => a - b)
    let length = arr.length
    if (length%2 === 0) {
        return (arr[length / 2 -1] + arr[length /2])/2.0
    } else {
        return (arr[(length+1)/2 -1])
    }
};
```
# 01 - Two Sum

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

## 暴力解 (Brute Force)

時間複雜度：$O(n^2)$

空間複雜度：$O(1)$

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  const length = nums.length;
  
  for (let i = 0; i < length; i++) {
    for (let j = i + 1; j < length; j++) {
      if (target - nums[i] === nums[j]) {
        return [i, j];
      }
    }
  }
  return [];
};
```

## 雜湊表（Hash Map）

### Two-pass

先將傳入的陣列遍歷一次，做成 `Map`，key: value 為 `nums[i]`: `i`，然後再對傳入的陣列遍歷一次，判斷各個值減掉 `target` 後是否存在於 `Map` 之中，有，回傳對應的答案；無，回傳空陣列。

時間複雜度：$O(n)$，遍歷陣列兩次，而雜奏表將查詢降到 $O(1)$

空間複雜度：$O(n)$

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  const length = nums.length;
  const hashMap = new Map();

  for (let i = 0; i < length; i++) {
    hashMap.set(nums[i], i)
  }
  
  for (let i = 0; i < length; i++) {
    const matchingValue = hashMap.get(target - nums[i]);

    // 注意：檢查是否使用相同元素 ex nums: [3,2,4] target: 6 output: [0, 0]
    if (matchingValue !== undefined && matchingValue !== i) {
      return [matchingValue, i];
    } else {
      hashMap.set(nums[i], i);
    }
  }  
  return [];
};
```

### One-pass

建立空的 `Map`，遍歷一次傳入的陣列，如果 target 減掉當前 `nums[i]` 的值不存在於 `Map`，則將其加到 `Map`，如果全部皆不存在，回傳空陣列；若有，回傳答案。

時間複雜度：$O(n)$，遍歷陣列一次，雜湊查詢花費 $O(1)$

空間複雜度：$O(n)$

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  const length = nums.length;
  const hashMap = new Map();
  
  for (let i = 0; i < length; i++) {
    const matchingValue = hashMap.get(target - nums[i]);

    if (matchingValue !== undefined) {
      return [matchingValue, i];
    } else {
      hashMap.set(nums[i], i);
    }
  }  
  return [];
};
```

## 也可使用陣列

時間複雜度：$O(n)$

空間複雜度：$O(n)$

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  const length = nums.length;
  const array = [];
  
  for (let i = 0; i < length; i++) {
    const diffValue = target - nums[i];
    const matchingIndex = array.indexOf(diffValue);

    if (matchingIndex !== -1) {
      return [matchingIndex, i];
    } else {
      array.push(nums[i]);
    }
  }  
  return [];
};
```
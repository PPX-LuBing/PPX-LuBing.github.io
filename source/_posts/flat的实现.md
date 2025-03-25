---
title: flat的实现
date: 2025-03-25 10:55:46
tags:
---

# 实现一个flat函数

### 递归实现

递归方法通过逐层遍历数组元素，遇到嵌套数组时递归展开，直到达到指定深度。

```jsx
function flat(arr, depth = 1) {
  const result = [];
  for (let i = 0; i < arr.length; i++) {
    if (i in arr) { // 跳过空槽
      const element = arr[i];
      if (Array.isArray(element) && depth > 0) {
        const nextDepth = depth === Infinity ? Infinity : depth - 1;
        result.push(...flat(element, nextDepth));
      } else {
        result.push(element);
      }
    }
  }
  return result;
}
```

### 迭代实现

迭代方法通过队列（广度优先）处理元素，避免递归导致的栈溢出问题。队列保存待处理的元素及其当前深度，按顺序展开子元素。

```jsx
function flat(arr, depth = 1) {
  const result = [];
  const queue = [];
  // 初始化队列，处理存在的元素
  for (let i = 0; i < arr.length; i++) {
    if (i in arr) {
      queue.push({ value: arr[i], depth: 0 });
    }
  }
  while (queue.length > 0) {
    const { value, depth: currentDepth } = queue.shift();
    if (Array.isArray(value) && currentDepth < depth) {
      // 展开子元素，按顺序加入队列
      for (let i = 0; i < value.length; i++) {
        if (i in value) {
          queue.push({ value: value[i], depth: currentDepth + 1 });
        }
      }
    } else {
      result.push(value);
    }
  }
  return result;
}
```

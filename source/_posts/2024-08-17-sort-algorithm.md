---
title: 排序算法
tags:
  - Algorithm
  - Sorting
categories:
  - 算法
abbrlink: d3afdbfa
date: 2024-08-17 09:40:46
updated: 2024-08-18 17:38:00
description: 本文主要记录本人所常用的排序算法
sticky: 1
swiper_index: 1
cover: https://tuchuang.voooe.cn/images/2024/08/17/lianggongchunri_1920x1080.webp
---

# ~~冒泡排序~~

{% tabs 分栏 %}

<!-- tab C++ -->

```cpp
void swap(int &a, int &b) {
    if (a == b) return;
    a ^= b;
    b ^= a;
    a ^= b;
}

void BubbleSort(int *arr, int size) {
    bool is_ordered;
    for (int i = 0; i < size - 1; ++i) {
        is_ordered = true;
        for (int j = 0; j < size - i - 1; ++j) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                is_ordered = false;
            }
        }
        if (is_ordered)
            return;
    }
}
```

<!-- endtab -->
<!-- tab JavaScript -->

```JavaScript
function bubbleSort(arr = [], len = arr.length) {
  let isOrdered;
  for (let i = 0;i < len - 1;++i) {
    isOrdered = true;
    for (let j = 0;j < len - i - 1;++j) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
        isOrdered = false;
      }
    }
    if (isOrdered) return;
  }
}
```

<!-- endtab -->

{% endtabs %}

# 快速排序

{% tabs 分栏 %}

<!-- tab C++ -->

```cpp
int Partition(int *arr, int low, int high) {
    int start = low, pivot = arr[low];
    while (low < high) {
        while (low < high && arr[high] >= pivot)
            --high;
        while (low < high && arr[low] <= pivot)
            ++low;
        // arr[high] < pivot and arr[low] > pivot
        swap(arr[low], arr[high]);
    }
    // low == high
    swap(arr[start], arr[low]);
    return low;
}

void QuickSort(int *arr, int low, int high) {
    if (low >= high)
        return;
    int pivot_key = Partition(arr, low, high);
    QuickSort(arr, low, pivot_key - 1);
    QuickSort(arr, pivot_key + 1, high);
}
```

<!-- endtab -->
<!-- tab JavaScript -->

```JavaScript
function Partition(arr = [], low = 0, high = arr.length - 1) {
  let start = low;
  let pivot = arr[low];
  while (low < high) {
    while (low < high && arr[high] >= pivot) --high;
    while (low < high && arr[low] <= pivot) ++low;
    // arr[high] < pivot and arr[low] > pivot
    [arr[low], arr[high]] = [arr[high], arr[low]];
  }
  // low == high
  [arr[start], arr[low]] = [arr[low], arr[start]];
  return low;
}

function quickSort(arr = [], low = 0, high = arr.length - 1) {
  if (low >= high) return;
  const pivot_key = Partition(arr, low, high);
  quickSort(arr, low, pivot_key - 1);
  quickSort(arr, pivot_key + 1, high);
}
```

<!-- endtab -->

{% endtabs %}

# 归并排序

{% tabs 分栏 %}

<!-- tab C++ -->

```cpp
void Merge(int *arr, const int low, const int mid, const int high) {
  int i = low, j = mid + 1, k = 0, *p = new int[high - low + 1];
  while (i <= mid && j <= high)
    p[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
  while (i <= mid)
    p[k++] = arr[i++];
  while (j <= high)
    p[k++] = arr[j++];
  for (i = low; i <= high; ++i)
    arr[i] = p[i - low];
  delete[] p;
  // 避免成为野指针
  p = nullptr;
}

void MergeSort(int *arr, const int low, const int high) {
  if (low >= high)
    return;
  const int mid = (low + high) >> 1;
  MergeSort(arr, low, mid);
  MergeSort(arr, mid + 1, high);
  Merge(arr, low, mid, high);
}
```

<!-- endtab -->

<!-- tab JavaScript -->

```JavaScript
function merge(arr = [], low = 0, mid = 0, high = arr.length - 1) {
  // 左半边起点
  let i = low;
  // 右半边起点
  let j = mid + 1;
  // 存放排好序部分的临时数组
  let p = [];
  // 临时数组游标
  let k = 0;
  while (i <= mid && j <= high) p[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
  // 若仅左边仍有剩余
  while (i <= mid) p[k++] = arr[i++];
  // 若仅右边仍有剩余
  while (j <= high) p[k++] = arr[j++];
  // 将排序后的部分写回原数组中对应位置
  for (i = low; i <= high; ++i) arr[i] = p[i - low];
  p = null;
}

function mergeSort(arr = [], low = 0, high = arr.length - 1) {
  if (low >= high) return;
  const mid = (low + high) >> 1;
  // 对范围[low, mid]部分排序
  mergeSort(arr, low, mid);
  // 对范围[mid + 1, high]部分排序
  mergeSort(arr, mid + 1, high);
  // 将左右两个单独有序的部分合并至整体有序的部分
  merge(arr, low, mid, high);
}
```

<!-- endtab -->

{% endtabs %}

# （大根）堆排序

&nbsp;&nbsp;&nbsp;&nbsp;在根节点下标为1的堆中，下标0留空，下标为i的节点的左右孩子（若存在）则分别为`2i`和`2i+1`。而当根节点下标为0时，虽然不满足上面的规则，但是仍然有类似的规律，可以先将全体下标`+1`转化为从1开始的情况，然后按照上面的规律计算其左右孩子的下标，再将得到的结果`-1`即可得到根节点下标为0的堆中左右孩子与父节点的关系。因此，从0开始存储的堆中,节点i的左右孩子分别为`[2(i + 1)] - 1`和`[2(i+1)+1] - 1`，也就是`2i + 1`和`2i + 2`。

&nbsp;&nbsp;&nbsp;&nbsp;下面的算法均以根节点下标为0的堆为标准。

{% tabs 分栏 %}

<!-- tab C++ -->

```cpp
void MaxHeapify(int *arr, int root, const int range) {
  for (int i = (root << 1) + 1; i < range; i = (i << 1) + 1) {
    if (i + 1 < range && arr[i] < arr[i + 1])
      ++i;
    if (arr[i] <= arr[root])
      return;
    swap(arr[i], arr[root]);
    root = i;
  }
}

void MaxHeapSort(int *arr, const int size) {
  for (int i = (size >> 1) - 1; i >= 0; --i)
    MaxHeapify(arr, i, size);
  for (int i = size - 1; i > 0; --i) {
    swap(arr[0], arr[i]);
    MaxHeapify(arr, 0, i);
  }
}
```

<!-- endtab -->

<!-- tab JavaScript -->

```JavaScript
function maxHeapify(arr = [], root = 0, range = arr.length) {
  for (let i = (root << 1) + 1; i < range; i = (i << 1) + 1) {
    // i 最初指向root的左孩子，后让其指向最大的孩子
    if (i + 1 < range && arr[i] < arr[i + 1]) ++i;
    // 最大的孩子都没有根大则表明无需调整
    // （下面的部分是已调整过的，未变化，满足大根堆标准）
    if (arr[i] <= arr[root]) return;
    // 孩子更大则交换，继续以交换值的孩子为根向下调整
    // （另一个孩子的值未发生变化，仍满足大根堆标准）
    [arr[i], arr[root]] = [arr[root], arr[i]];
    root = i;
  }
}

function maxHeapSort(arr = [], len = arr.length) {
  // 构建大根堆
  for (let i = (len >> 1) - 1; i >= 0; --i) maxHeapify(arr, i, len);
  // 从后向前，依次将根（首位）与最后未排序元素交换位置后，从根向下（后）调整为大根堆
  for (let i = len - 1; i > 0; --i) {
    [arr[0], arr[i]] = [arr[i], arr[0]];
    maxHeapify(arr, 0, i);
  }
}
```

<!-- endtab -->

{% endtabs %}

# 鸽巢排序

{% tabs 分栏 %}

<!-- tab C++ -->

```cpp
int PigeonholeSort(int *arr, const int size) {
    int max, min, i;
    for (i = 1, max = min = arr[0]; i < size; ++i) {
        if (arr[i] > max) {
            max = arr[i];
        } else if (arr[i] < min) {
            min = arr[i];
        }
    }
    int *pigeonhole = new int[max - min + 1]();
    if (!pigeonhole)
        return 1;
    for (i = 0; i < size; ++i) {
        ++pigeonhole[arr[i] - min];
    }
    i = 0;
    for (int k = 0; i <= max - min; ++i) {
        while (pigeonhole[i]) {
            arr[k++] = i + min;
            --pigeonhole[i];
        }
    }
    delete[] pigeonhole;
    pigeonhole = nullptr;
    return 0;
}
```

<!-- endtab -->

<!-- tab JavaScript -->

```JavaScript
function pigeonholeSort(arr = [], len = arr.length) {
  let i = 1;
  let max, min;
  for (max = min = arr[0]; i < len; ++i) { // O(n)
    if (arr[i] > max) {
      max = arr[i];
    } else if (arr[i] < min) {
      min = arr[i];
    }
  }
  let pigeonhole = Array(max - min + 1).fill(0);
  if (!pigeonhole) return -1;
  for (i = 0; i < len; ++i) { // O(n)
    ++pigeonhole[arr[i] - min];
  }
  i = 0;
  for (let k = 0; i <= max - min; ++i) { // O(range)
    while (pigeonhole[i]) {
      --pigeonhole[i];
      arr[k++] = i + min;
    }
  }
  pigeonhole = null;
  return;
}
```

<!-- endtab -->

{% endtabs %}

&nbsp;&nbsp;&nbsp;&nbsp;显然鸽巢排序的时间复杂度为O(n+range)，由于需要申请值的范围大小的空间作为鸽巢，所以空间复杂度为O(range)。
&nbsp;&nbsp;&nbsp;&nbsp;特点如下：

- 当待排序值的范围远小于个数n时，时间复杂度趋于O(n)；
- 由上可知，该算法对<abbr title="小范围内、大量">密集型</abbr>数据排序时的效率特别高，但对分布稀疏的数据排序效率则比较低；
- 由于需要给范围内的每一个整数一个鸽巢，因此，仅适用于为整数排序；

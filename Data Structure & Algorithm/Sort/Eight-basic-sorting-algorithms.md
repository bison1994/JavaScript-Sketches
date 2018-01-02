### 选择排序 Selection Sort

- 基本思路：不断从剩余数组中选出最小的数，然后移到前面
- O(n^2)。需要约 N^2 / 2 次比较，与最多 N - 1 次交换
- 运行时间与输入无关，给定 N 的情况下，无论何种输入所需时间均相等
- 数据交换量最小

```js
function f (arr) {
  for (var i = 0, len = arr.length; i < len - 1; i++) {
    var min = i;
    for (var j = i + 1; j < len; j++) {
      if (arr[j] < arr[min]) {
        min = j
      }
    }
    if (min !== i) {
      var temp = arr[i];
      arr[i] = arr[min];
      arr[min] = temp;
    }
  }
  return arr
}
```


### 冒泡排序
- 基本原理：依次比较相邻的两个数，通过交换将较大的数不断冒泡移动至末端。下一轮扫描的终点是上一轮扫描时最后一次发生交换的位置
- 最好情形：正序，仅需 n - 1 次比较
- 最坏情形：反序，需要平方级的比较和平方级的交换

```js
function bubble (arr) {
  var index = arr.length - 1;
  while (index > 0) {
    var mark = 0;
    for (var j = 0; j < index; j++) {
      if (arr[j] > arr[j + 1]) {
        var temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
        mark = j
      }
    }
    index = mark;
  }
  return arr
}
```


### 插入排序 Insertion Sort

- 基本思路：依次将每个数与左侧相邻的数比较，如果小于左侧的数，则交换位置
- 时间与输入相关。需 N - 1 ~ N^2 / 2 次比较，0 ~ N^2 / 2 次交换
- 与冒泡排序的异同
  + 复杂度一致
  + 插入排序的平均比较次数比冒泡排序更少
  + 每一轮循环，冒泡排序最右侧就是最终排好序的，而插入排序最左侧并不是最终排好序的
  + 冒泡排序每一次冒泡的部分都是未排序的部分，插入排序每一次都在将数据冒泡到已排序的部分
  + 冒泡排序每次循环的长度越来越小，插入排序每次循环的长度越来越大

```js
function f (arr) {
  for (var i = 1, len = arr.length; i < len; i++) {
    for (var j = i; j > 0; j--) {
      if (arr[j] < arr[j - 1]) {
        var temp = arr[j];
        arr[j] = arr[j - 1];
        arr[j - 1] = temp;
      } else {
        break;
      }
    }
  }
  return arr
}
```


### 希尔排序 Shell Sort

基本思路：希尔排序是对插入排序的一种改进，通过增大交换的跨越度，渐进式的提高输入的有序性，将增长级数限制在平方级以内

```js
function f (arr) {
  var len = arr.length;
  var h = Math.floor(len / 3);
  while (h >= 1) {
    for (var i = h; i < len; i += h) {
      for (var j = i; j > 0; j -= h) {
        if (arr[j] < arr[j - h]) {
          var temp = arr[j];
          arr[j] = arr[j - h];
          arr[j - h] = temp;
        } else {
          break;
        }
      }
    }
    h = Math.floor(h / 3)
  }
  return arr
}
```


### 归并排序 Merge Sort

基本思路：将输入分为两半分别排序，然后合并，该方法可以递归的进行，将需要排序的数组长度不断二分至最小，从而降低排序的消耗

```js
// mid 分隔的两个数组必须是排好序的
function merge (arr, lo, mid, hi) {
  var copy = [];
  var k = mid + 1;
  for (var i = lo; i <= hi; i++) {
    copy[i] = arr[i];
  }
  for (i = lo; i <= hi; i++) {
    if (lo > mid) {
      arr[i] = copy[k++];
    } else if (k > hi) {
      arr[i] = copy[lo++];
    } else if (copy[lo] < copy[k]) {
      arr[i] = copy[lo++];
    } else {
      arr[i] = copy[k++];
    }
  }
  return arr
}

/**
 * 自顶向下
 * 递归的最深处，是对二元数组进行归并
 */
function sort (arr, lo, hi) {
  if (lo >= hi) return;
  var mid = lo + Math.floor((hi - lo) / 2);
  sort(arr, lo, mid);
  sort(arr, mid + 1, hi);
  return merge(arr, lo, mid, hi)
}

/**
 * 自底向上
 */
function sort (arr) {
  var len = arr.length;
  var max, mid;
  for (var i = 2; i / 2 < len; i *= 2) {
    for (var j = 0; j < len; j += i) {
      if (j + i - 1 >= len) {
        max = len - 1;
      } else {
      	max = j + i - 1;
      }
      mid = j + i / 2 - 1;
      merge(arr, j, mid, max);
    }
  }
  return arr
}
```


### 快速排序 Quick Sort

- 平均复杂度：线性对数级
- 基本思路：将每一个数都移动到这样一个位置：其左边的数都不大于它，其右边的数都不小于它，此时整个数列是有序的
- 实现上述状态的步骤是：首先随机选取一个数，将其左边大于它的数都移到其右边，将其右边小于它的数都移到其左边，上述过程递归的进行，直到每个数都到达其正确的位置
- 快速排序的潜在问题是递归过深可能导致栈溢出

```js
function quickSort (arr, lo, hi) {
  if (lo >= hi) return;
  var i = lo;
  var j = hi;
  var key = arr[lo];
  while (true) {
    while (j > i) {
      if (arr[j] < key) {
        arr[i] = arr[j];
        arr[j] = key;
        break;
      }
      j--
    }
    while (i < j) {
      if (arr[i] > key) {
        arr[j] = arr[i];
        arr[i] = key;
        break;
      }
      i++
    }
    if (i >= j) break;
  }
  quickSort(arr, lo, i - 1);
  quickSort(arr, i + 1, hi);

  return arr
}
```


### 堆排序
- 基本思路
  + 构造堆
  + 将最大元素 a[1] 与末尾元素交换
  + 利用下沉恢复堆有序
  + 重复前两个步骤

```js
function sink (arr, k, N) {
  while (2 * k < N) {
    var j = 2 * k;
    if (j + 1 < N && arr[j + 1] > arr[j]) {
      j += 1
    }
    if (arr[k] < arr[j]) {
      var temp = arr[k];
      arr[k] = arr[j];
      arr[j] = temp;
      k = j
    } else {
      break;
    }
  }
}

function sort (arr) {
  var N = arr.length;
  // 构造堆
  for (var k = Math.floor(N / 2); k >= 1; k--) {
    sink(arr, k, N)
  }
  // 排序
  while (N-- > 1) {
    var temp = arr[1];
    arr[1] = arr[N];
    arr[N] = temp;
    sink(arr, 1, N)
  }

  return arr
}
```


### 基数排序

分别按各分位数排列
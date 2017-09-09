
### 选择排序 Selection Sort
基本思路：不断从剩余数组中选出最小的数，然后移到前面
- 平方级。需要约 N^2 / 2 次比较，与 N - 1 次交换
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
    var temp = arr[i];
    arr[i] = arr[min];
    arr[min] = temp;
  }
  return arr
}
```

### 插入排序 Insertion Sort
基本思路：依次将每个数与左侧相邻的数比较，如果小于左侧的数，则交换位置
- 时间与输入相关。需 N - 1 ~ N^2 / 2 次比较，0 ~ N^2 / 2 次交换

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
      arr[i] = copy[k];
      k += 1;
      continue;
    }
    if (k > hi) {
      arr[i] = copy[lo];
      lo += 1;
      continue;
    }
    if (copy[lo] < copy[k]) {
      arr[i] = copy[lo];
      lo += 1;
    } else {
      arr[i] = copy[k];
      k += 1;
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
快排的基本思路是：将每一个数都移动到这样一个位置：其左边的数都不大于它，其右边的数都不小于它，此时整个数列是有序的。
<br>
实现上述状态的步骤是：首先随机选取一个数，将其左边大于它的数都移到其右边，将其右边小于它的数都移到其左边，上述过程递归的进行，直到每个数都到达其正确的位置。
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
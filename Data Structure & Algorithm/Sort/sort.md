### 分类
- 内部排序
  + 插入排序
    - 直接插入排序（1）
    - 希尔排序（2）
  + 选择排序
    - 简单选择排序（3）
    - 堆排序（4）
  + 交换排序
    - 冒泡排序（5）
    - 快速排序（6）
  + 归并排序（7）
  + 基数排序（8）
- 外部排序

### 排序涉及的问题
- 复杂度
  + Time Complexity
    - Best/Worst/Averavge number of comparisons
    - Best/Worst/Average number of swap
  + Memory Complexity
    - [数据量比内存更大的情况](http://www.jianshu.com/p/b8faa1affe17)
- Comparable
  + 对于任意数据类型，能够排序的前提是可比较
  + 比较的实现可能是多维度的，即 comparable 接口可能具有多种实现
  + 比较的依据可能是多维度的，即可能依据不同的属性进行排序
- 不可变性
  + 键的数据类型应该是不可变的
- 稳定性 Stability
  + 排序后，同类数据的相对位置不变
- 代码量  
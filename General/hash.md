### What is hash
- hash 算法（哈希、散列）用于将任意长度的二进制值映射为较短的固定长度的二进制值（哈希值）
- 特点
  + 单向性。只有加密，没有解密，不可逆
  + 唯一性。同样的数据映射结果相同，不同的数据映射结果不同
  + 抗冲撞
- 哈希值长度固定，是一个有限集合，而输入值长度不固定，是一个无限集合，理论上一定会出现多个输入值对应同一个哈希值的情况。因此哈希算法的要点是确保在计算上要找到具有相同哈希值的不同原文极其困难

<br>

### What is hash table

哈希表的设计源于高速查找的需求，其基本思想是将数据的 hash 值作为内存地址，因此直接计算或读取 hash 值就可以查找到数据

> In computing, a hash table (hash map) is a data structure which implements an associative array abstract data type, a structure that can map keys to values. A hash table uses a hash function to compute an index into an array of buckets or slots, from which the desired value can be found.

根据散列函数 f(k) 和处理碰撞的方法将一组关键字映射到一个有限的连续的地址集（区间）上，并以关键字在地址集中的“像”作为记录在表中的存储位置，这种表便称为散列表，这一映射过程称为散列造表或散列，所得的存储位置称散列地址

哈希表的好处是当原始数据较大时，我们可以用哈希算法处理得到定长的哈希值 key，那么这个 key 相对原始数据要小得多。我们就可以用这个较小的数据集来做索引，达到快速查找的目的

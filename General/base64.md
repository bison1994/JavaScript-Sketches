### 简介

base64 是基于 64 个字符的编码，每个字符可以表示一个 6 位二进制数，用于编码二进制数据（单字节）

设计 base64 的目的主要是为了解决：如何在仅支持字符串的场景下使用二进制数据的问题。为什么是 64 字符而不是 128 个字符？[解释](https://stackoverflow.com/a/3538079)

64 字符分别是：26 个小写字母、26 个大写字母、10 个数字、+、=

> [同类的 Binary-to-Text 编码](https://en.wikipedia.org/wiki/Binary-to-text_encoding)

<br>

### 编码过程

- 将待转换的字符串每三个字节分为一组，每个字节占8bit，那么共有24个二进制位
- 将上面的24个二进制位每6个一组，共分为4组
- 在每组前面添加两个0（注：因为计算机的基本处理单位是字节），每组由6个变为8个二进制位，总共32个二进制位，即四个字节
- 根据 Base64 编码对照表获得对应的值
- 字节有余数的情况
    + 两个字节：两个字节共16个二进制位，依旧按照规则进行分组。此时总共16个二进制位，每6个一组，则第三组缺少2位，用0补齐，得到三个 Base64 编码，第四组完全没有数据则用“=”补上
    + 一个字节：一个字节共8个二进制位，依旧按照规则进行分组。此时共8个二进制位，每6个一组，则第二组缺少4位，用0补齐，得到两个Base64编码，而后面两组没有对应数据，都用“=”补上

> 参考：https://blog.csdn.net/wo541075754/article/details/81734770

显然，base64 并不是一种加密方法，更不是压缩方法，编码后体积会增大约 33%

<br>

### 相关 api

- window.btoa 编码
- window.atob 解码

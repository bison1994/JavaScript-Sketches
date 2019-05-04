### Blob
	
- Blob（二进制大对象）是一种数据格式，文件是 Blob 的一个子类
- ES5 引入 Blob 对象，用于操作二进制数组
-	基于 Blob 衍生出了相关的 API，如 File 对象、FileList 对象、FileReader 对象和 URL 对象
-	Blob 提供一个 slice 方法，用于对二进制数据进行切割

```js
function () {
  var blob = target.files[0]
  const BYTES_PER_CHUNK = 1024 * 1024 // 1MB chunk sizes.
  const SIZE = blob.size
  var start = 0
  var end = BYTES_PER_CHUNK
  while (start < SIZE) {
    upload(blob.slice(start, end)) // 上传文件
    start = end
    end = start + BYTES_PER_CHUNK
  }
}
```

### FileList

有两种获取该对象的方式，一是上传表单的属性，二是拖放事件的事件对象的属性


### File

File 对象是 FileList 数组的成员。该对象有以下属性：
- name	文件名
- size	文件大小，单位为字节
- type	文件的MIME类型
- lastModified	文件的上次修改时间，格式为时间戳
- lastModifiedDate	文件的上次修改时间，格式为Date对象实例


### FileReader

FileReader 用于把文件内容读入内存。它的参数是 File 对象或 Blob 对象

```js
var reader = new FileReader()
reader.readAsDataURL(input.files[0])
reader.onload = function() {
  img.src = reader.result
}
```

- 属性
  + error
  + readyState
  + result
- 方法
  + .readAsDataURL()
  + .readAsText()
  + .readAsArrayBuffer()
  + .readAsBinaryString()
  + .abort()
- 事件
  + .onload
  + .onprogress
  + .onerror
  + .onabort


### URL

URL 对象用于将 File 或 Blob 数据转化为 URL

```js
var url = window.URL.createObjectURL(file)
```

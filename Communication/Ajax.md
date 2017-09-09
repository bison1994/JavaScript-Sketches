### Asynchronous JavaScript and XML

- Asynchronous。ajax 由独立的线程执行，不会阻塞主线程
- JavaScript。ajax 由 JS 发起。但 JS 对请求头的操作是受限的
- XML。响应的数据格式，目前被 JSON 取代

### 实现 ajax 的方式
- <img src="">
  + 返回图片格式数据
  + 不受同源限制
  + 常用于仅发送不接受的场景
- <iframe src="">
  + 返回 HTML 文档
  + 受限于同源政策
- <script src="">
  + 返回 js 文件
  + 不受同源限制
- XMLHttpRequest
  + XMLHttpRequest 对象的每一个实例支持一个独立的请求/响应对
  + 老版的实现仅支持文本类型的数据，且受同源限制
  + 新版本（XHR2，IE10 以下不支持，IE8/9 可用 XDomainRequest）扩展了许多功能
    - 可以设置HTTP请求的时限
    - 可以使用FormData对象管理表单数据
    - 可以上传文件
    - 可以跨域请求（CORS）
    - 可以获取服务器端的二进制数据
    - 可以获得数据传输的进度信息

[XHR2 参考](http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)


### request 的四个组成部分
一个http请求由四个部分组成：
	-方法（必须）
	-地址（必须）
	-请求头（可选）
	-请求主体（可选）

#### 方法


#### 地址

#### 请求头

#### 请求主体
由于 Http 只能传输特定格式的数据，因此数据的发送和接收都需要相应的编码和解码的工作，有的工作是由浏览器实现的，而有的工作需要人工完成。

##### 1.表单型数据编码
将名与值进行URL编码（十六进制转义码）并以=连接，名值对之间以&连接。
	该数据格式的 MIME 类型：application/x-www-form-urlencoded
	使用 POST 方法提交数据时，需设置 Content-Type 为该值。
	可在 URL 中使用该种数据类型进行 GET 只读查询。

##### 2.JSON编码
	xhr.setRequestHeader("Content-Type", "application/json");
	xhr.send(JSON.stringify(data));

##### 3. 上传文件
	XHR2支持Blob的数据格式
	var input = …… // input是一个file类型的表单元件
	input.addEventListener("change", function() {
		var file = this.files[0]; 
		var xhr = new XMLHttpRequest();
		xhr.open("POST", url);
		xhr.send(file);
	})

##### 4. 混合数据
	使用FormData对象一次性上传混合多种格式的数据（multipart/form-data）。
	var data = {……};
	var formdata = new FormData();
	for (name in data) {
	  formdata.append(name, data[name]);
	}
	var xhr = new XMLHttpRequest();
	xhr.open("POST", url);
	xhr.send(formdata);
	使用jQuery：
	$.ajax({
      url: '/url',
      type: 'POST',
      cache: false,
      data: formData,
      processData: false,
      contentType: false
	})


### ajax 的执行过程








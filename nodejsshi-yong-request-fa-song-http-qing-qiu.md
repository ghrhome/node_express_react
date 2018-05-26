# nodejs使用request发送http请求

在nodejs的开发中，有时需要后台去调用其他服务器的接口，这个时候，就需要发送HTTP请求了。有一个简单的工具可以用，[Simplified HTTP request client](https://github.com/request/request)，可以比较方便的模拟请求。

# 安装 {#安装}

```
npm install --save request
```

# 使用 {#使用}

最简单的GET请求，用法如下：

```
var request = require('request');
request('http://www.baidu.com', function (error, response, body) {
  if (!error && response.statusCode == 200) {
    console.log(body) // Show the HTML for the baidu homepage.
  }
})
```

# POST application/json {#post-applicationjson}

```
request({
    url: url,
    method: "POST",
    json: true,
    headers: {
        "content-type": "application/json",
    },
    body: JSON.stringify(requestData)
}, function(error, response, body) {
    if (!error && response.statusCode == 200) {
    }
}); 
```

# POST application/x-www-form-urlencoded {#post-applicationx-www-form-urlencoded}

```
request.post({url:'http://service.com/upload', form:{key:'value'}}, function(error, response, body) {
    if (!error && response.statusCode == 200) {
    }
})
```

# POST multipart/form-data {#post-multipartform-data}

```
var formData = {
    // Pass a simple key-value pair
    my_field: 'my_value',
    // Pass data via Buffers
    my_buffer: new Buffer([1, 2, 3]),
    // Pass data via Streams
    my_file: fs.createReadStream(__dirname + '/unicycle.jpg'),
};
request.post({url:'http://service.com/upload', formData: formData}, function (error, response, body) {  
    if (!error && response.statusCode == 200) {
    }
})
```

如上所示，formData可以直接放key-value格式的数据，也可以放buffer，或者是通过流描述的文件。










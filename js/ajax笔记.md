* 对ajax的理解
	* ajax是什么?
		* AJAX（Asynchronous JavaScript and XML）
		* 是一种浏览器端不用刷新整个页面就可以与服务器端通信的技术
		* 它不是新技术, 而是一种由多种技术组合的技术
		* 包括Javascript、HTML和CSS、DOM、XML和JSON、XMLHttpRequest
	* ajax的应用
		* 检查用户是否可用
		* 三级联动
		* 地图
		* 搜索列表提示
	* 比较传统web请求与ajax请求
		* 传统web请求 : 
			* 同步请求: 发2个请求, 只完全完全处理完第1个后, 才能发第2个请求
			* 响应数据: 带数据的页面
			* 谁来发送的请求: 一般的http请求模块
		* ajax请求 :
			* 异步请求: 发2个请求, 可以瞬间将2个请求都发送出去(此时并没有得到响应), 通过监听回调函数来接收响应数据
			* 响应数据: json/xml/html标签数据
			* 谁来发送的请求: ajax引擎模块
	* ajax请求的过程(图)
	    * 创建xmlhttprequest对象, 并设置用于接收响应数据的监听回调, 并在回调中读取响应数据, 更新局部页面
	    * xmlhttprequest.send()发送异步请求
	    * ajax接收到这个命令后, 就会发送一个http请求
	    * 服务器接收到请求, 对请求进行处理, 并返回一个响应数据(json/xml/html标签)
	    * ajax引擎接收到响应数据后的处理:
	        * 将响应数据保存到request对象上(responseXML/responseText)
	        * 调用request对象的onreadystatechange()
* ajax开发
    * 原生API:
        var request = new XmlHttpRequest();
        request.onreadystateChange = function(){
            if(request.readyState==4&&request.status==200) {
                var result = request.responeText/XML;
                //更新局部页面
            }
        }
        request.open('POST/GET', url, true/false)
        request.setRequestHeader('Content-Type', '......'); //标识对请求体数据进行编码处理
        request.send(body);
    * jQuery包装API
        $.ajax({
            type : 'POST/GET',
            url : 'url',
            data : {}/string,
            success : function(msg){},
            error : function(){},
            dataType : 'json/xml/html'  //响应数据
        });
        
        $.get(url, data, function(msg){}, 'json')
        $.post(url, data, function(msg){}, 'json')
        
        $.getJSON(url, data, function(msg){})
        
* 后台路由:
    router.get/post('path', function(req, res, next){
        //1. 获取请求参数数据
        var xxx = req.query.xxx
        var xxx = req.body.yyy
        //2. 处理数据
            //逻辑处理
        //3. 返回响应数据
        res.end/send/json(result);
    })
    
* ajax跨域请求
    * 问题说明: 浏览器基于同源策略(安全策略)，不允许跨域请求(ajax)
    * 跨域: 协议，域名，端口号, 只要有一个不同就是跨域
    * 解决办法
        * jsonp
            * 基本原理:
                * 本质不是ajax请求
                * 动态创建一个<script>, 指定src属性为请求的url, 浏览器会自动发送对url的get类型的一般HTTP请求
                * 服务器接收到请求, 处理请求, 返回是一个函数调用的字符串, 并将数据(json格式)作为实参传入
                * 浏览器接收到响应数据, 解析它, 就会去执行函数调用(之前就已经定义好的一个回调函数)
                * 在回调函数中, 得到返回的json数据, 读取内部数据进行处理
            编码(jQuery):
                * 浏览器端
                    * $.getJSON('url?callback=?', function(obj/arr){})
                * 服务器端
                    * var callback = req.query.callback
                    * var resultJson = '{}/[]';
                    * res.send(callback+'('+resultJson+')');
        * cors
            * 服务端, 向响应中添加一个特别的响应头: 'Access-Controll-Allow-Origin':'*'
            * 存在浏览器兼容的问题
* 前端js模板
    * 作用: 将ajax请求得到数据渲染到提前定义好的标签模板生成html标签结构, 插入到页面的指定标签中
    * 场景: ajax请求返回数据比较大时
    * 优点: 编码简洁, 支行效率高
    * 基本语法: 使用特殊结构({{}})来实现标签与js的嵌套
    * 基本原理: 使用正则表达式匹配: 看到<> 就是标签, 一旦看到{{}}就当作js解析
* jQuery的文档结构
    *图
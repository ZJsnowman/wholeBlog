---
title: 对Gson的一些理解
date: 2016-11-29 18:38:49
tags: [Android]
---

自己对Gson的一些理解<!-- more -->


### Gson是什么
在解释什么是Gson之前先要说一下 JSON 。JSON 是一种文本形式的数据交换格式，它比XML更轻
量、比二进制容易阅读和编写，调式也更加方便。而Gson就是方便我们解析和生成Json的一个工具。类似
的还有很多，比如 Jackson、FastJson 等。

### Gson的基本用法。
Gson提供了 fromJson() 和toJson() 两个直接用于解析和生成的方法，前者实现反序列化，后者实现了
序列化。同时每个方法都提供了重载方法。
在Volley中，虽然已经提供了JsonObjectRequest和JsonArrayRequest。但是我们拿到JsonObjec
或者JsonArray还是需要自己在手动转换成实体类，这个繁琐的过程就可以通过Gson很好的为我们自动完成。
我们可以自己去定义一个GsonRequest，自己来解析网络返回的数据。
```java
@Override
   protected Response<T> parseNetworkResponse(NetworkResponse response) {
       try {
           String jsonString = new String(response.data,  // 将二进制数据包装成string
                   HttpHeaderParser.parseCharset(response.headers));
           return Response.success(mGson.fromJson(jsonString,mClass),  //再利用Gson转换成实体类
                   HttpHeaderParser.parseCacheHeaders(response));
       } catch (UnsupportedEncodingException e) {
           return Response.error(new ParseError(e));
       }
   }
```
网络返回的真实数据是二进制的byte数组，通过String的包装，我们拿到jsonString（String里面存放
的是一个json数据），然后再通过Gson.fromJson()方法，将json反序列化为实体类mClass.


### Gson的注意事项
这里记录一下自己的一些疑惑。
实体类和服务器返回的一些对比：
1. 类型不同
2. 名字不同
3. 本地有定义，服务器没定义
4. 本地没有定义，服务器有返回。

自己做了一些实验，结果如下，有误的地方，还望指出。
1. 会存在类型转换，比如服务器返回的boolean，本地定义的是int，那么拿到的就会是0或者1
2. null，不会报错。定义错的那个变量就是空。
3. 本地多定义的那部分都是null,false,0(参数默认值)，不会报错。
4. 不会报错，只是丢掉了服务器返回的那部分数据。

总结，本地和后台必须严格保持一致，所以我们最好利用第三方工具，来帮我们通过json数据生成对应的实体
类，比如GsonFormat.如果没有对应上，不会报错，但会出现数据不准确（类型不同），数据丢失（2，3，4）。

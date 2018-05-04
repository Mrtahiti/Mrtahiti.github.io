---
layout:     post
title:      Android网络通信
subtitle:   使用安卓与网络通信小demo
date:       2018-05-04
author:     TH
header-img: img/post-bg-and.jpg
catalog: true
tags:
    - Android
---



### Android网络通信

---

[TOC]

-----

#### Http协议介绍

##### 基本介绍

> hypertext transfer protocol（超文本传输协议），TCP/IP 协议的一个应用层协议，用于 定义 WEB 浏览器与 WEB 服务器之间交换数据的过程。客户端连上 web 服务器后，若想获得 web 服务器 中的某个 web 资源，需遵守一定的通讯格式，HTTP 协议用于定义客户端与 web 服务器通迅的格式。

#####  连接过程

1. 用户点击浏览器上的 URL(超链接)，Web 浏览器与 Web 服务器建立连接

2. 建立连接后，客户端发送请求给服务器，请求的格式为: 统一资源标识符 (URL)+ 协议版本号 (一般是 1.1)+MIME 信息 (多个消息头)+ 一个空行 

3. 服务端收到请求后，给予相应的返回信息，返回格式为: 协议版本号 + 状态行 (处理结果) + 多个信息头 + 空行 + 实体内容 (比如返回的 HTML)

4. 客户端接收服务端返回信息，通过浏览器显示出来，然后与服务端断开连接；当然如果中途 某步发生错误的话，错误信息会返回到客户端，并显示，比如：经典的 404 错误！

   

   示意图如下：

   ![](https://ws1.sinaimg.cn/large/d8b30b42ly1fqz5huy5ozj20dh08n751.jpg)

#### Android基本的网络通信

一般有两种方式：**HttpURLConnection** 和 **HttpClient**.

在此只介绍**HttpURLConnection** .

##### 代码示例

```java
private void sendRequestWithHttpURLConnection() {
        // 开启线程来发起网络请求
        new Thread(new Runnable() {
            @Override
            public void run() {
                HttpURLConnection connection = null;
                BufferedReader reader = null;
                try {
                    //HttpURLConnection使用实例
                    URL url = new URL("http://www.baidu.com");
                    connection = (HttpURLConnection) url.openConnection();//实例化
                    connection.setRequestMethod("GET");//从服务器获取数据
                    connection.setConnectTimeout(8000);//连接超时设置
                    connection.setReadTimeout(8000);
                    
                    InputStream in = connection.getInputStream();
                    // 下面对获取到的输入流进行读取
                    reader = new BufferedReader(new InputStreamReader(in));
                    StringBuilder response = new StringBuilder();
                    String line;
                    while ((line = reader.readLine()) != null) {
                        response.append(line);
                    }
                    showResponse(response.toString());
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    if (reader != null) {
                        try {
                            reader.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                    if (connection != null) {
                        connection.disconnect();
                    }
                }
            }
        }).start();
    }

private void showResponse(final String response) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                // 在这里进行UI操作，将结果显示到界面上
                responseText.setText(response);
            }
        });
    }
```

##### 代码分析

1. HttpURLConnection实例化

```java
					URL url = new URL("http://www.baidu.com");
					connection = (HttpURLConnection) url.openConnection();//实例化
					connection.setRequestMethod("GET");//从服务器获取数据
					connection.setConnectTimeout(8000);//连接超时设置
					connection.setReadTimeout(8000);
```

2. 数据读取与写入

```java
                    InputStream in = connection.getInputStream();
                    // 下面对获取到的输入流进行读取
                    reader = new BufferedReader(new InputStreamReader(in));
                    StringBuilder response = new StringBuilder();
                    String line;
                    while ((line = reader.readLine()) != null) {
                        response.append(line);
                    }
                    showResponse(response.toString());

```

##### 进阶

使用**OkHttp**替代原生的**HttpURLConnection** 

```java
OkHttpClient client = new OkHttpClient();
Request request = new Request.Builder().url(address).build();//address为字符型url链接
Response response = client.newCall(request).execute();//获取数据
String responseData = response.body().string();
```

代码更改为：

```java
private void sendRequestWithOkHttp() {
        new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    OkHttpClient client = new OkHttpClient();
                    Request request = new Request.Builder()
                            .url("http://www.baidu.com")
                            .build();
                    Response response = client.newCall(request).execute();
                    String responseData = response.body().string();
                    showResponse(responseData);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
```



#### 进入培训

##### 目标：使用安卓获取Bing每日一图

我们在程序里具备哪些功能?

##### 工具准备

在build.gradle中添加

```java
compile 'com.squareup.okhttp3:okhttp:3.7.0'
compile 'com.squareup.okio:okio:1.12.0'
compile 'com.github.bumptech.glide:glide:3.7.0'
```

本次需要用到Glide、Okhttp3.

##### 简单的代码实现

1. 使用封装类

   ```java
   package com.example.myapplication.util;
   
   import okhttp3.OkHttpClient;
   import okhttp3.Request;
   
   public class HttpUtil {
   
       public static void sendOkHttpRequest(String address, okhttp3.Callback callback) {
           OkHttpClient client = new OkHttpClient();
           Request request = new Request.Builder().url(address).build();
           client.newCall(request).enqueue(callback);
       }
   
   }
   
   ```

2. 初始化

    ```java
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    bingPicImg =  findViewById(R.id.bing_pic_img);
    ```

3. 图片下载	

    ```java
    String requestBingPic = "http://guolin.tech/api/bing_pic";
            HttpUtil.sendOkHttpRequest(requestBingPic, new Callback() {
                @Override
                public void onResponse(Call call, Response response) throws IOException {
                    final String bingPic = response.body().string();
                    SharedPreferences.Editor editor = PreferenceManager.getDefaultSharedPreferences(MainActivity.this).edit();
                    editor.putString("bing_pic", bingPic);
                    editor.apply();
                                }
    ```

4. 图片加载	

    ```java
    //切换到主线程，更新UI元素                
    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            Glide.with(MainActivity.this).load(bingPic).into(bingPicImg);//bingPicImg是一个imageview的id
                        }
                    });
    
    ```

5. 判断是否存在缓存（选作）	

    ```java
    SharedPreferences prefs = PreferenceManager.getDefaultSharedPreferences(this);
            String bingPic = prefs.getString("bing_pic", null);
            if (bingPic != null) {
                Glide.with(this).load(bingPic).into(bingPicImg);
            } else {
                loadBingPic();
            }
    ```

    

##### 总体代码实现

一起动手吧！！！
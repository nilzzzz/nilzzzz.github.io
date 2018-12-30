---
layout: post
title: StringEntity， UrlEncodedFormEntity() ，MultipartEntity 的区别
tag: http
---
### 案例描述
> 近期，在POST数据是，用UrlEncodedFormEntity()这个方法传给后台，后台报错，说是无法解析json内容，不是以{开头。
> 
>  检查发现用UrlEncodedFormEntity()方法组织的数据发送到服务器是普通的键值对，不是json，所以后台无法接受。
> 后来改用 StringEntity()方法组织数据，数据的形式就非常自由了，可以组织成自己想要的任何形式，问题解决。


### StringEntity， UrlEncodedFormEntity() ，MultipartEntity 的区别

> **UrlEncodeFormEntity**会将参数以key1=value1&key2=value2的键值对形式发出。类似于传统的application/x-www-form-urlencoded表单上传
>
> **StringEntity**可以自己指定ContentType，而默认值是
> text/plain，数据的形式就非常自由了，可以组织成自己想要的任何形式，一般用来存储json数据
> 
> **MultipartEntity**文件上传用到，类似于表单的类型为multipart/form-data

### 三种方法的使用
1. UrlEncodedFormEntity
```
    List<NameValuePair> pairs = new ArrayList<NameValuePair>(); 
    pairs.add(new BasicNameValuePair("key", "value"));                      
    httpPost.setEntity(new UrlEncodedFormEntity(pairs, HTTP.UTF_8)); 
```

2. StringEntity
```
    JSONObject json = new JSONObject();  
    json.put("key", "value");                        
    httpPost.setEntity(new StringEntity(json.toString(), HTTP.UTF_8)); 
```

3. MultipartEntity
```

    MultipartEntity entity = new MultipartEntity();
     /* 上传文本，后边new StringBody(text,Charset.forName(CHARSET))为参数值，其实就是正常的值转换成utf-8的编码格式  */
    entity.addPart("key",new StringBody(text, Charset.forName("UTF-8")));
    // 上传音频文件  
    entity.addPart("audio", new FileBody(new File("path"), "audio/*"));  
    // 上传图片  
    entity.addPart("fileimg", new FileBody(new File("path"), "image/*"));
```


### 巴拉巴拉

    "Maturity is when you stop complaining and making excuses,and start making changes."


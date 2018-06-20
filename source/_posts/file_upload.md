---
title: 文件上传随记
date: 2018-04-02 10:01:30
tags:
	- fetch
  	- 前端
categories: 前端
---
## 前言
>多文件上传

项目之前采用的是element-ui官方的文件上传组件，但在实际开发的过程中，发现该组件虽然能够选择多个文件进行上传，但是却不能同时上传(文件只能发送一次http请求，上传一次)。
最后采用 POST FormData的方式进行上传。

## Fetch FormData

首先获取文件对象的数组(多文件的情况下，单文件就不需要遍历添加了),
随后实例FormData对象，将文件对象数组添加到FormData对象中，
再以fetch body属性上传就行了

``` js
let fileData = new FormData();
filesArray.map(item=>{
    fileData.append('file',item.raw)
})        
let fetchOptionsForFile={
    method:"POST",
    body:fileData
}
fetch(`${ipConfig}/api/Function/MFileImport`,fetchOptionsForFile).then(response=>response.json()).then(json=>{console.log(json);})
```
## 文件列表上传

通过选择文件夹，取得文件夹内的文件列表然后以POST FormData的方式进行上传。

上传配置只需在`input type=file`加上`webkitdirectory`属性即可。
``` html
<input ref="fileList"  type="file" webkitdirectory v-on:change ="uploadFileList"/>
```
`注:webkitdirectory属性目前仅支持webkit内核的浏览器。`

``` js
let fileList = this.$refs.fileList.files
```
取得文件列表`fileList`后循环添加到`fileData`对象就行了。
## End
后面再补充吧。

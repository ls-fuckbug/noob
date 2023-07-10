# 网页

网页一般由三部分组成，分别是 HTML（超文本标记语言）、CSS（层叠样式表）和 JavaScript（简称“JS”动态脚本语言），它们三者在网页中分别承担着不同的任务。

- HTML 负责定义网页的内容  

- CSS 负责描述网页的布局  

- JavaScript 负责网页的行为  


# HTML

HTML 英文全称是 Hyper Text Markup Language，中文译为“超文本标记语言”，专门用来设计和编辑网页。

## HTML元素

一般情况下，一个 HTML 标签由开始标签、属性、内容和结束标签组成，标签的名称不区分大小写，但大多数属性的值需要区分大小写  

	HTML 元素以开始标签起始  
	HTML 元素以结束标签终止  
	元素的内容是开始标签与结束标签之间的内容   
	某些 HTML 元素具有空内容（empty content）  
	空元素在开始标签中进行关闭（以开始标签的结束而结束）  
	大多数 HTML 元素可拥有属性  

## 常用HTML标签  
```
标题标签 <h1>  
段落标签 <p>  
超链接标签 <a>  
图片标签 <img>  
表格标签 <table>  
列表标签 <ul> <ol> <dl>
	有序列表： <ol> + <li> list term  
	无序列表： <ul> + <li> list term
	自定义列表: <dl> + <dt> definition term + <dd> definition description  
表单标签 <form>  


```

## HTML属性 attr  

```
href 属性可以为 <a> 标签提供链接地址；  
src 属性可以为 <img> 标签提供图像的路径；  
style 属性可以为几乎所有标签定义 CSS 样式。  
id 属性 唯一标识   
class 属性不必唯一标识  
title 属性对标签内容描述  

```



# DOM

DOM 是 W3C（万维网联盟）的标准。

DOM 定义了访问 HTML 和 XML 文档的标准  

	"W3C 文档对象模型 （DOM） 是中立于平台和语言的接口，它允许程序和脚本动态地访问和更新文档的内容、结构和样式。"

W3C DOM 标准被分为 3 个不同的部分：

- 核心 DOM - 针对任何结构化文档的标准模型  

- XML DOM - 针对 XML 文档的标准模型  

- HTML DOM - 针对 HTML 文档的标准模型  


根据 W3C 的 HTML DOM 标准，HTML 文档中的所有内容都是节点：

- 整个文档是一个文档节点  

- 每个 HTML 元素是元素节点  

- HTML 元素内的文本是文本节点  

- 每个 HTML 属性是属性节点  

- 注释是注释节点  

## 一些常用的 HTML DOM 方法：

- getElementById(id) - 获取带有指定 id 的节点（元素） 

- appendChild(node) - 插入新的子节点（元素） 

- removeChild(node) - 删除子节点（元素） 

一些常用的 HTML DOM 属性：

- innerHTML - 节点（元素）的文本值 

- parentNode - 节点（元素）的父节点  

- childNodes - 节点（元素）的子节点  

- attributes - 节点（元素）的属性节点  









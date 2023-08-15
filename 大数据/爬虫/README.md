# css selector  

```
.class     根据calss选取  
#id        根据id选取  
p          选取所有p标签元素  
div,p      选取所有div元素和p元素  
div.op      选取所有class为op的div元素  
div p       选取div元素内的所有p元素   
div>p       选取所有父级是div元素的p元素  
div+p       选取所有在div元素后的第一个p元素  
[target]    选取所有带target属性的元素  
[target=x]  选取所有target属性等于x的元素  
[target~=x] 选取所有target属性包含x的元素  
[target|=x] 选取所有target属性等于x或以x开头的元素  
:link       选择所有未访问的链接  
:visited    选择所有已访问的链接  
*           选择所有
:not([target])   选取不带target属性的元素
 
```




# 查看网页cookie

console输入 document.cookie 即可显示当前页面cookie信息



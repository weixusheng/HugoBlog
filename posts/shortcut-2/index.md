# Chrome删除重复书签



删除重复书签
<!--more-->

Bookmarks clean up 可以合并同名文件夹，删除失效书签，删除重复书签

太好用了！！！！！！！

由于Bookmarks clean up 展示完重复书签，不能自动选，而需要用户手动选中
如果书签量很大的话，就需要大量的工作了，下面代码功能就是自动选中其他书签
可以自己改代码，根据书签特征来选择

Step: 
打开[Bookmarks clean up](https://chrome.google.com/webstore/detail/bookmarks-clean-up/oncbjlgldmiagjophlhobkogeladjijl/related)
Find duplicated bookmarks 
打开Chrome 控制台
控制台输入下面代码

```js
var dupArray = document.getElementsByClassName("duplicate card")
for ( var i = 0; i <dupArray.length; i++){
    var items = dupArray[i].getElementsByClassName("list-group-item");
    if (items.length >= 2) {
        // 默认设置第一个以外的item选中
        for (var j = 1; j < items.length; j++) {
            var item = items[j]
            var checkbox = item.getElementsByClassName("custom-control-input")
            console.log(checkbox)
            checkbox.item(0).click()
        }
    } 
}
```

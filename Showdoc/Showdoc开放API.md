Showdoc开放API
===


## 开放API

**介绍**

showdoc开放文档编辑的API，供使用者更加方便地操作文档数据。利用开放API，你可以自动化地完成很多事。例如下面列举三个典型的应用场景：

*   假如你的团队有很多现成的文档资料，但这些文档资料大多以word的形式存在。如果人工复制粘粘，工作量会有点多。此时你可以写一个自动脚本，从文件中生成文档，然后通过showdoc的开放api批量地把文档更新进去。

*   你是个后端程序员，你想在写完代码后，文档能自动更新到showdoc。为了实现这个效果，你可以写一个脚本程序，根据你项目代码的结构，自动生成文档数据，然后通过showdoc的开放api自动更新。

*   你有很多笔记教程，存在笔记软件或者网站博客中，但你想把它们归类在一起供人们查阅。 这时你可以写程序批量导入showdoc

开放API提供的是一种自动更新的能力，使用场景不止上面所说的。更多场景请发挥你的想象力吧！

出于数据安全考虑，showdoc提供的API只能用于添加/编辑文档，无法删除文档。如果要使用更高级的操作，请登录showdoc网页完成。下面是API文档的技术细节，仔细阅读参数说明有助于你理解怎么使用API。阅读明白后可用在线API测试工具进行测试：[http://runapi.showdoc.cc/](http://runapi.showdoc.cc/)

**请求URL：**

*   如果你使用www.showdoc.cc ,则请求URL为

    `https://www.showdoc.cc/server/api/item/updateByApi`

*   如果你使用开源版的showdoc ,则请求url为

    `http://xx.com/server/index.php?s=/api/item/updateByApi`

**请求方式：**

*   POST

**参数：**

| 参数名 | 必选 | 类型 | 说明 |
| --- | --- | --- | --- |
| api_key | 是 | string | api_key，认证凭证。登录showdoc，进入具体项目后，点击右上角的”项目设置”-“开放API”便可看到 |
| api_token | 是 | string | 同上 |
| cat_name | 否 | string | 可选参数。当页面文档处于目录下时，请传递目录名。当目录名不存在时，showdoc会自动创建此目录 |
| cat_name_sub | 否 | string | 可选参数。当页面文档处于更细分的子目录下时，请传递子目录名。当子目录名不存在时，showdoc会自动创建此子目录 |
| page_title | 是 | string | 页面标题。请保证其唯一。（或者，当页面处于目录下时，请保证页面标题在该目录下唯一）。当页面标题不存在时，showdoc将会创建此页面。当页面标题存在时，将用page_content更新其内容 |
| page_content | 是 | string | 页面内容，可传递markdown格式的文本或者html源码 |
| s_number | 否 | number | 可选，页面序号。默认是99。数字越小，该页面越靠前 |

**成功时的返回示例**
```json
     { 
         "error_code" : "0", 
         "data" : { 
              "page_id" : "101921", 
              "author_uid" : "0", 
              "author_username" : "from_api", 
              "item_id" : "12011", 
              "cat_id" : "25619", 
              "page_title" : "api测试", 
              "page_comments" : "", 
              "page_content" : "我是测试内容555455666666666", 
              "s_number" : "99", 
              "addtime" : "1484444430" 
         } 
     }
```
**认证失败时的返回示例**
```json
     { 
         "error_code" : "10306", 
         "error_message" : "api_key 或 api_token 不匹配" 
     }
```
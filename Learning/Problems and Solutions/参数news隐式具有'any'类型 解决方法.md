---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

代码
```
function changePage(news) {
  router.push({
      name:'detail',
      // path:'news/detail',
      query:{
          id:news.id,
          title:news.title,
          content:news.content,
      }
  })
}
```

方法1 在报错参数后加上:any
```
function changePage(news:any) {
  router.push({
      name:'detail',
      // path:'news/detail',
      query:{
          id:news.id,
          title:news.title,
          content:news.content,
      }
  })
}
```
方法2 新写接口，定义所有参数类型
```
interface NewsInter {
    id:string,
    title:string,
    content:string,
}
function changePage(news:NewsInter) {
    router.push({
        name:'detail',
        // path:'news/detail',
        query:{
            id:news.id,
            title:news.title,
            content:news.content,
        }
    })
}
```
---
tags:
  - Blog
  - Experiences
  - Working
---
#Done 
```
//用来获取当前年月
const handleTimeOld = () => {  
    let date = new Date()
    let year = date.getFullYear()
    let month = date.getMonth() + 1
    // split = '-'
    return [year, month].join('-')
}
```
---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

结论：我写的tableData没加reactive
顺便放上尝试过的几个方法：
[增加key属性](https://blog.csdn.net/A7_A8_A9/article/details/107957419#1%EF%BC%9A%20data%E6%95%B0%E6%8D%AE%E6%9B%B4%E6%96%B0%E8%A7%86%E5%9B%BE%E4%B8%8D%E6%9B%B4%E6%96%B0%E7%9A%84%E9%97%AE%E9%A2%98%E3%80%82)
[也是增加key](https://www.cnblogs.com/tanweiwei/p/16813605.html)
[安装版本更低的element-plus](https://blog.csdn.net/Ultravioletrays/article/details/131429435)
[其他方法](https://blog.csdn.net/Raken12/article/details/112612870)
```
<template>
    <el-table :data="tableData" @change="console.log('changed',tableData);">
        <el-table-column prop="area" label="area"></el-table-column>
        <el-table-column prop="date" label="date"></el-table-column>
        <el-table-column prop="dayForecast" label="dayForecast"></el-table-column>
    </el-table>

</template>

<script setup name="Model" lang="ts">
import axios from 'axios';
import { reactive } from 'vue';
const tableData = reactive([])
async function getData() {
    try {
        const promise1 = await axios({url: 'http://hmajax.itheima.net/api/weather', params: {city: '110100'}})
        tableData.push({
            area: promise1.data.data.area,
            date: promise1.data.data.date,
            dayForecast: promise1.data.data.dayForecast[0].weather,
        })       
        const promise2 = await axios({url: 'http://hmajax.itheima.net/api/weather', params: {city: '310100'}})
        tableData.push({
            area: promise2.data.data.area,
            date: promise2.data.data.date,
            dayForecast: promise2.data.data.dayForecast[0].weather,
        })
        const promise3 = await axios({url: 'http://hmajax.itheima.net/api/weather', params: {city: '440100'}})
        tableData.push({
            area: promise3.data.data.area,
            date: promise3.data.data.date,
            dayForecast: promise3.data.data.dayForecast[0].weather,
        })
        const promise4 = await axios({url: 'http://hmajax.itheima.net/api/weather', params: {city: '440300'}})
        tableData.push({
            area: promise4.data.data.area,
            date: promise4.data.data.date,
            dayForecast: promise4.data.data.dayForecast[0].weather,
        })
        console.log(tableData);
        
    } catch(err) {
        console.dir(err)
    }
    tableData.push({
            area: 'promise1.data.data.area',
            date: 'promise1.data.data.date',
            dayForecast: 'promise1.data.data.dayForecast[0].weather',
        })
}

// //卸载当前版本
// npm uninstall element-ui
// //安装指定版本
// npm i element-ui@2.4.8 -S --legacy-peer-deps
getData()

console.log('aaa',tableData);

</script>
```
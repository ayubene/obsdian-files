---
tags:
  - Blog
  - Experiences
  - Working
---
#Done 
在做打印小票的需求
首先是安装这两个插件
```
npm install jsbarcode --save
npm install print-js--save
```
引入
```
import printJS from 'print-js'
import JsBarcode from 'jsbarcode'
```
使用 写一个函数
```
const repairdemo = () => {
  
  setTimeout(() => {
    // 这个是生成条码 ，`#barcode`是设置的img或svg标签的id，state.currentClickItemId是要生成条码的字符串，{}内则是一些配置项
    JsBarcode(`#barcode`, state.currentClickItemId, {
      fORMat: "CODE128",//条形码的格式
      width: 1,//线宽
      height: 40,//条码高度
      lineColor: "#000",//线条颜色
      // displayValue: false,//是否显示文字
      fontSize: 14,
      margin: 2//设置条形码周围的空白区域
    })
  }, 200);

  setTimeout(() => {
    console.log('执行打印');
    // 这个是打印，'receipt'是对应标签的id
    printJS({
      printable: 'receipt', // 元素id,不支持多个
      type: 'html',
      scanStyles: false, //不适用默认样式
      style: '@page{size:auto;margin: 0cm 1cm 0cm 1cm;}',//去除页眉页脚
      css: '/styles/receiptPrint.css' //css url
    })
  }, 1000);
}
```
注意：如果css写在vue文件中是不生效的，要写在行内或者独立css文件，配置在printJS中
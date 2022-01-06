---
title: 前端导出PDF
date: 2020-03-31 14:00:48
tags: [pdf, html2canvas, jspdf]
---

项目中遇到需要前端直接导出 PDF 的需求，具体实现方式如下

<!-- more -->

## 原理

1. 把 dom 转换成 canvas 并作为图片导出
2. 把图片切分，并渲染到 PDF 上

## 采用开源库

- [html2canvas](https://github.com/niklasvh/html2canvas)
- [jspdf](https://github.com/MrRio/jsPDF)

## 具体代码

```js
// 导出页面为PDF格式
import html2Canvas from "html2canvas";
import JsPDF from "jspdf";

export { html2pdf, html2Canvas, JsPDF };
export default async function html2pdf(el, title = +new Date()) {
  const canvas = await html2Canvas(el, {
    allowTaint: true
  });
  const A4 = [595.28, 841.89]; // A4纸的尺寸是210mm×297mm。当设定的分辨率是72像素/英寸时，A4纸的尺寸的图像的像素是595×842。
  // 需要打印的内容尺寸
  let contentWidth = canvas.width;
  let contentHeight = canvas.height;
  // 图片的尺寸
  const imgWidth = A4[0];
  let imgHeight = (A4[0] / contentWidth) * contentHeight;
  // PDF的尺寸
  let pageHeight = (contentWidth / A4[0]) * A4[1];
  // 初始打印内容参数
  let leftHeight = contentHeight;
  let positionY = 0; // y轴偏移量

  let pageData = canvas.toDataURL("image/jpeg", 1.0);
  let PDF = new JsPDF("", "pt", "a4");
  //当内容未超过pdf页面显示的范围，无需分页
  if (leftHeight < pageHeight) {
    PDF.addImage(pageData, "JPEG", 0, 0, imgWidth, imgHeight);
  } else {
    while (leftHeight > 0) {
      PDF.addImage(pageData, "JPEG", 0, positionY, imgWidth, imgHeight);
      leftHeight -= pageHeight;
      positionY -= A4[1];
      //如果还存在内容继续添加新页
      if (leftHeight > 0) {
        PDF.addPage();
      }
    }
  }
  PDF.save(`${title}.pdf`);
}
```

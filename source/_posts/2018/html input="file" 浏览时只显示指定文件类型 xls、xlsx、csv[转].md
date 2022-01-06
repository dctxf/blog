---
title: html input="file" 浏览时只显示指定文件类型 xls、xlsx、csv[转]
date: Sat Mar 17 2018 00:37:44 GMT+0800 (CST)
updated: Wed Mar 21 2018 14:04:45 GMT+0800 (CST)
comments: 1
categories: code
tags: [html, input, 前端]
permalink: /html-input-file-xls
---

页面文件上传类型控制

事例：

```html
<input id="fileSelect" type="file" accept=".csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel" />
```

<!-- more -->

## Valid Accept Types:

- For CSV files (.csv), use:

      	```
      	<input type="file" accept=".csv" />
      	```

- For Excel Files 2003-2007 (.xls), use:

      	```
      	<input type="file" accept="application/vnd.ms-excel" />
      	```

- For Excel Files 2010 (.xlsx), use:

      	```
      	<input type="file" accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" />
      	```

- For Text Files (.txt) use:

      	```
      	<input type="file" accept="text/plain" />
      	```

- For Image Files (.png/.jpg/etc), use:

      	```
      	<input type="file" accept="image/*" />
      	```

- For HTML Files (.htm,.html), use:

      	```
      	<input type="file" accept="text/html" />
      	```

- For Video Files (.avi, .mpg, .mpeg, .mp4), use:

      	```
      	<input type="file" accept="video/*" />
      	```

- For Audio Files (.mp3, .wav, etc), use:

      	```
      	<input type="file" accept="audio/*" />
      	```

- For PDF Files, use:

      	```
      	<input type="file" accept=".pdf" />
      	```

## NOTE:

If you are trying to display Excel CSV files (.csv), do NOT use:

- `text/csv`
- `application/csv`
- `text/comma-separated-values (works in Opera only).`

If you are trying to display a particular file type (for example, a `WAV` or `PDF`), then this will almost always work...

```
 <input type="file" accept=".FILETYPE" />
```

原文：

<http://stackoverflow.com/questions/11832930/html-input-file-accept-attribute-file-type-csv>

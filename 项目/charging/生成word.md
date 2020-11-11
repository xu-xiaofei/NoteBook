### 1.freemarker生成WORD文件

+ 基本思路。使用word 编辑好ftl模板，然后填充数据
+  两个注意要点：
    +  一是：插入WORD中的图片大小会按照模板中的图片大小自动进行放大或缩小，所以建议最好按照事先规定的格式来上传固定图片尺寸；
    -  二是：最终生成的WORD文档在打开时有时会出现提示文档错误，解决方案:
        模板用office2003编辑(比如WORD2010高版本保存为WORD2003即可，千万不要用WPS处理！)，之后另存为选择2003WORD-XML，而不是直接保存为WORD-XML）。


### 2. 导入图片主要要点

   -  标签<w:binData> 之间不应该有任何空格之类的，不要换行，否则引用不到图片
    ```xml
        <w:binData w:name="wordml://02000006.jpg" xml:space="preserve">${imageList[5]}</w:binData>
    ```
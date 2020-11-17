
## 1. 基本思路：
   - html -> PDF
   - word -> PDF
   - 直接生成PDF，但是复杂

## 2.依赖
    ```java
            <!--用于将html转为PDF输出-->
            <dependency>
                <groupId>com.itextpdf</groupId>
                <artifactId>html2pdf</artifactId>
                <version>${itextpdf.version}</version>
            </dependency>
            <!-- free mark 模板引擎，用于静态化生成报表html-->
            <dependency>
                <groupId>org.freemarker</groupId>
                <artifactId>freemarker</artifactId>
                <version>${freemarker.version}</version>
            </dependency>
            <!-- 用于画图 -->
            <dependency>
                <groupId>org.jfree</groupId>
                <artifactId>jfreechart</artifactId>
                <version>${jfree.version}</version>
            </dependency>
    ```

## 3.PDF的生成

    基本组建 Itext7 + free marker + JFree char

    Itext5 对于html 中的CSS支持并不好，甚至不能解析，网上的很多教程都是基于Itext5的XML Worker 组件，已经被废弃。
    最终，我们决定从头开始重写iText，同时考虑到HTML + CSS转换的要求。这产生了iText 7。在iText 7之上，我们创建了几个附加组件，在这种情况下，最重要的附加组件是pdfHTML。

    1. 如何分页
        @page 写在html 的header里面
        
    2. ftl 模板
        - 标签必须闭合
        -  必须是标准的html 文件，需要文件头

    3. 代码演示
    ```java
        
        /**
        * @Created by xiaofei.xu
        * @Date 2020/10/26
        * Description:
        */

        import com.itextpdf.html2pdf.ConverterProperties;
        import com.itextpdf.html2pdf.HtmlConverter;
        import com.itextpdf.kernel.geom.PageSize;
        import com.itextpdf.kernel.pdf.PdfDocument;
        import com.itextpdf.kernel.pdf.PdfWriter;
        import com.itextpdf.layout.Document;
        import com.itextpdf.layout.font.FontProvider;

        import java.io.File;
        import java.io.FileOutputStream;
        import java.io.IOException;

        /**
        * The type Java to pdf html free marker.
        *
        * @author xiaofei.xu
        */
        public class PdfUtils {

            /**
            * 根据生成的html文件的String字符串，来生成PDF文件
            *
            * @param htmlString the html string
            * @throws IOException the io exception
            */
            public static void createPDFFile(String htmlString,String destFilePath) throws IOException {

                File destFile = new File(destFilePath);
                if (destFile.exists()){
                    destFile.delete();
                }
                FileOutputStream fileOutputStream = new FileOutputStream(new File(destFilePath));

                PdfWriter pdfWriter = new PdfWriter(fileOutputStream);
                ConverterProperties properties = new ConverterProperties();
                properties.setCharset("utf-8");
                FontProvider font = new FontProvider();
            /* 这code 是为了添加字体，如果字体库中已经有了中文，那么就支持中文了*/
                font.addSystemFonts();
                properties.setFontProvider(font);

                PdfDocument pdfDocument = new PdfDocument(pdfWriter);
                /*For setting the PAGE SIZE*/
                pdfDocument.setDefaultPageSize(new PageSize(PageSize.A4));

                Document document = HtmlConverter.convertToDocument(htmlString, pdfDocument, properties);
                document.close();
            }
        }
    ```


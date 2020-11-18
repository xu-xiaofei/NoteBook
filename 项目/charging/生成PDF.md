
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

## 4. 代码演示

  - 4.2 构建HTML

    ```java
    import com.meinergy.chargingpile.common.exception.GlobalException;
    import freemarker.template.Configuration;
    import freemarker.template.Template;
    import freemarker.template.TemplateException;
    import lombok.extern.slf4j.Slf4j;

    import javax.validation.Valid;
    import java.io.IOException;
    import java.io.StringWriter;

    /**
    * @Created by xiaofei.xu
    * @Date 2020/10/29
    * Description:
    */
    @Slf4j
    public class FreeMarkerUtils {

        /**
        * templates/bus/费用测算模板.ftl
        * 使用free marker 模板和引擎生成内容的String字符串
        *
        * @param dataModel                the data model
        * @param templateRelativeDir the template relative dir  模板相对class的路径
        * @param templateName        the template name  模板的名字.ftl
        * @return string
        * @throws IOException       the io exception
        * @throws TemplateException the template exception
        */
        public static String generateContent(@Valid Object dataModel, String templateRelativeDir, String templateName) {
            try {
                // 第一步：创建一个Configuration对象。
                Configuration configuration = new Configuration(Configuration.getVersion());
                // 创建一个Writer对象。
                StringWriter out = new StringWriter();
                // 第二步：设置模板文件所在的路径。
                configuration.setClassForTemplateLoading(FreeMarkerUtils.class, templateRelativeDir);
                // 第三步：设置模板文件使用的字符集。
                configuration.setDefaultEncoding("utf-8");
                // 第四步：加载一个模板，创建一个模板对象。
                Template template = configuration.getTemplate(templateName);
                // 第五步：调用模板对象的process将数据塞入模板中。
                template.process(dataModel, out);
                out.flush();
                //返回处理好的字符串
                String contentString = out.toString();
                out.close();
                return contentString;
            } catch (Exception e) {
                log.error("Failed to generate file", e);
                throw new GlobalException("生成文件失败");
            }
        }

    ```
    -  4.1 HTML转PDF

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



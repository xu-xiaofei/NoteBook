## 有无返回值都在controller层进行控制

### 1. 有返回值的，压缩流


```java
    public static ResponseEntity downloadResponse(String zipFileName, List<Pair<String, String>> csvFiles) {
        try (
                ByteArrayOutputStream byteArrayOutputStream = compressZip(csvFiles);
                InputStream inputStream = new ByteArrayInputStream(byteArrayOutputStream.toByteArray());
        ) {
            InputStreamResource resource = new InputStreamResource(inputStream);
            HttpHeaders headers = new HttpHeaders();
            headers.add("Cache-Control", "no-cache, no-store, must-revalidate");
            headers.add("Pragma", "no-cache");
            headers.add("Expires", "0");
            headers.add("content-disposition", "attachment;filename=" + URLEncoder.encode(zipFileName, "utf-8"));

            return ResponseEntity.ok()
                    .headers(headers)
                    .contentType(MediaType.parseMediaType("application/octet-stream"))
                    .body(resource);
        } catch (IOException e) {
            e.printStackTrace();
        }

        return ResponseEntity.ok().body(StringUtils.EMPTY);
    }

    /**
     * 将多个文件压缩到指定输出流中
     *
     * @return ByteArrayOutputStream
     */
    public static ByteArrayOutputStream compressZip(List<Pair<String,String>> csvFiles) {
        //-- 包装成ZIP格式输出流
        try (ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
             ZipOutputStream zipOutStream = new ZipOutputStream(byteArrayOutputStream)) {
            // -- 设置压缩方法
            zipOutStream.setMethod(ZipOutputStream.DEFLATED);
            //-- 将多文件循环写入压缩包
            for(Pair<String,String> csvFile:csvFiles){
                //-- 添加ZipEntry，并ZipEntry中写入文件流
                zipOutStream.putNextEntry(new ZipEntry(csvFile.getLeft()));
                zipOutStream.write(csvFile.getRight().getBytes());
                zipOutStream.closeEntry();
            }
            zipOutStream.flush();
            return byteArrayOutputStream;
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }

```

### 2. 无返回值，写入response 
   -  重要的是controller层不能有返回值，得使用viod，否则RestController 注解会导致一个方法两次返回，从而报错

```java

    /**
     * <pre>
     * 浏览器下载文件时需要在服务端给出下载的文件名，当文件名是ASCII字符时没有问题
     * 当文件名有非ASCII字符时就有可能出现乱码
     *
     * 这里的实现方式参考这篇文章
     * http://blog.robotshell.org/2012/deal-with-http-header-encoding-for-file-download/
     *
     * 最终设置的response header是这样:
     *
     * Content-Disposition: attachment;
     *                      filename="encoded_text";
     *                      filename*=utf-8''encoded_text
     *
     * 其中encoded_text是经过RFC 3986的“百分号URL编码”规则处理过的文件名
     * </pre>
     * @param response
     * @param filename
     * @return
     */
 /*   public static void setFileDownloadHeader(HttpServletResponse response, String filename) {
        String headerFormat = "attachment;filename=\"%s\";filename*=utf-8''%s";
        String encodedFileName = encodeURIComponent(filename);
        String headerValue = String.format(headerFormat, encodedFileName, encodedFileName);
        response.setHeader("Content-Disposition", headerValue);
        response.setCharacterEncoding("UTF-8");
        *//*如果出现了文件损坏，不妨设置一下长度，可以有效防止未关闭的流中有脏数据，导致文件损坏，所以设置长度比较好*//*
        //response.setHeader("Content-Length", String.valueOf(tempIn.available()));
        // 或者再尝试写完以后进行response的flush和close
    }*/

    public static void setFileDownloadHeader(HttpServletResponse response, String filename) {
        try {
            /*Must set content type */
            //response.setContentType("application/vnd.ms-excel");
            String fileDownloadName = URLEncoder.encode(filename, "UTF-8");
            response.setCharacterEncoding("utf-8");
            response.setHeader("Content-Disposition", "attachment; filename=" + fileDownloadName);
        } catch (UnsupportedEncodingException e) {
            log.error("Failed to encode file name");
        }
    }

    /*重置response*/
    public static void resetResponseHeader(HttpServletResponse response){
        response.reset();
        response.setContentType("application/json");
        response.setCharacterEncoding("utf-8");
//        Map<String, String> map = new HashMap<String, String>();
//        map.put("status", "failure");
//        map.put("message", "下载文件失败" + e.getMessage());
//        response.getWriter().println(JSON.toJSONString(map));
    }

    /**
     * <pre>
     * 符合 RFC 3986 标准的“百分号URL编码”
     * 在这个方法里，空格会被编码成%20，而不是+
     * 和浏览器的encodeURIComponent行为一致
     * </pre>
     * @param value
     * @return
     */
    @SneakyThrows
    public static String encodeURIComponent(String value) {
        return URLEncoder.encode(value, "utf-8").replaceAll("\\+", "%20");
    }

    /*静默关闭response*/
    public static void closeResponseQuietly(HttpServletResponse response) {
        try {
            response.getOutputStream().flush();
            response.getOutputStream().close();
        } catch (Exception e) {
            log.error("Failed to ");
        }
    }


```
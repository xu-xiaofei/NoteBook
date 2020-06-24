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
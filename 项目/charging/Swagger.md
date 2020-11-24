### 1. Swageer 上传文件的MultiPartFile 的写法
```java
    @ApiOperation("公交站点统计数据导出")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "stationId", value = "充电站", required = true, dataType = "Long", paramType = "path"),
            @ApiImplicitParam(name = "startDate", value = "开始时间", required = true, dataType = "String", paramType = "query"),
            @ApiImplicitParam(name = "endDate", value = "结束时间", required = true, dataType = "String", paramType = "query"),
            @ApiImplicitParam(name = "fleet", value = "车队名", required = false, dataType = "String", paramType = "query"),
            @ApiImplicitParam(name = "ownerVendorId", value = "车辆运行单位", required = false, dataType = "Long", paramType = "query")
    })
    @PostMapping(value = "/export/{stationId}")
    public BaseResult<String> exportReport(HttpServletResponse response,
                                           @ApiParam(name = "file",value = "对比文件.xlsx", required = true) @RequestBody MultipartFile file,
                                           @PathVariable Long stationId,
                                           @RequestParam @DateTimeFormat(pattern = "yyyy-MM-dd") Date startDate,
                                           @RequestParam @DateTimeFormat(pattern = "yyyy-MM-dd") Date endDate,
                                           @RequestParam(required = false) String fleet,
                                           @RequestParam(required = false) Long ownerVendorId) {
        busStationCompareService.exportBusStationCompareData(response, file, stationId, startDate, endDate, fleet, ownerVendorId);
        return BaseResult.SUCCESS;
    }
```
实例图：

 ![swagger pict](/Pictures/Swagger_pic_1.png)
  
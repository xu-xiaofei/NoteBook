### 1. 执行脚本

    实践当中在select 的标签内写sql脚本是成立的，但是一个脚本内执行多个select会导致有多个返回值，这样的返回值需要resultMap = "string,string,baseMap"之类的映射Map来做    

### 2.     
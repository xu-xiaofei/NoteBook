
1. Cookie 的载入
```java
@RequestMapping(value = "/setCookies",method = RequestMethod.GET)
    public  String setCookies(HttpServletResponse response){
        //HttpServerletRequest 装请求信息类
        //HttpServerletRespionse 装相应信息的类
        Cookie cookie=new Cookie("sessionId","CookieTestInfo");
        response.addCookie(cookie);
        return "添加cookies信息成功";
    }
```

2. Cookie的获取

```java
@RequestMapping(value = "/getCookies",method = RequestMethod.GET)
public  String getCookies(HttpServletRequest request){
    //HttpServletRequest 装请求信息类
    //HttpServletRespionse 装相应信息的类
 //   Cookie cookie=new Cookie("sessionId","CookieTestInfo");
    Cookie[] cookies =  request.getCookies();
    if(cookies != null){
        for(Cookie cookie : cookies){
            if(cookie.getName().equals("sessionId")){
                return cookie.getValue();
            }
        }
    }
 
   return  null;
}

```

或者@CookieValue

```java
@RequestMapping("/testCookieValue")
public String testCookieValue(@CookieValue("sessionId") String sessionId ) {
   //前提是已经创建了或者已经存在cookie了，那么下面这个就直接把对应的key值拿出来了。
   System.out.println("testCookieValue,sessionId="+sessionId);

    return "SUCCESS";

}

```
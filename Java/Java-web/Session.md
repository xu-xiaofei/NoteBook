
### Session and Cookies

1. Session 

 - Spring 的框架自带了一个session  org.springframework.boot.web.servlet.server.Session;

 - springboot 的web依赖 spring-boot-starter-web ，其子依赖与tomcat包，tomcat包里有常见ServletRequRequestest,httpSession。
   ServletRequRequestest 的子类HttpServletRequest中包含了Cookies 和HttpSession，HttpSession已经是顶级接口

 - JMS 中需要用Session来传递消息  


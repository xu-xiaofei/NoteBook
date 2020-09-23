### request util 工具类

```java
import com.envisioniot.trc.backendservice.constant.AttributeType;
import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.StringUtils;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;
import org.springframework.web.util.WebUtils;

import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import java.util.Objects;

/**
 * The type Request util.
 *
 * @author: xiaofei.xu
 */
@Slf4j
public class RequestUtil {

    /**
     * Gets current request.
     *
     * @return the current request
     */
    public static HttpServletRequest getCurrentRequest() {
        HttpServletRequest request = ((ServletRequestAttributes) RequestContextHolder.getRequestAttributes()).getRequest();
        log.info("Current thread's request is {}", request);
        return request;
    }

    /**
     * Gets attribute.
     *
     * @param attributeName the attribute name
     * @param type          the type
     * @return the attribute
     */
    public static String getAttribute(String attributeName, AttributeType type) {
        HttpServletRequest request = getCurrentRequest();
        switch (type) {
            case PARAM:
            case COOKIE:
                return resolverCookie(request, attributeName);
            case HEADER:
                return request.getHeader(attributeName);
        }

        return StringUtils.EMPTY;
    }

    /**
     * Resolver cookie string.
     *
     * @param request    the request
     * @param cookieName the cookie name
     * @return the string
     */
    public static String resolverCookie(HttpServletRequest request, String cookieName) {
        Cookie cookieValue = WebUtils.getCookie(request, cookieName);
        if (!Objects.isNull(cookieName)) {
            return cookieValue.getValue();
        }
        log.warn("Failed to resolver target cookie named {}", cookieName);
        return StringUtils.EMPTY;
    }

    /*
     * hard code  because of  no more an elegant way
     */
    public static String localeToLanguage(String locale) {
        switch (locale) {
            case "zh-CN":
            case "zh_CN":
                locale = "JA";
                break;
            case "en-US":
            case "en_US":
                locale = "EN";
                break;
        }
        return locale;
    }
}
```
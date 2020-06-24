```java

import lombok.extern.slf4j.Slf4j;
import org.springframework.http.*;
import org.springframework.util.Assert;
import org.springframework.util.LinkedMultiValueMap;
import org.springframework.util.MultiValueMap;
import org.springframework.web.client.RestTemplate;

import java.util.Arrays;
import java.util.Map;

/**
 * The type Rest template utils.
 *
 * @author: xiaofei.xu
 */
@Slf4j
public class RestTemplateUtils {

    /**
     * Post template t.
     *
     * @param <T>          the type parameter
     * @param <E>          the type parameter
     * @param restTemplate the rest template
     * @param url          TSDB Service url
     * @param body         Which you want to transfer to TSDB service
     * @param type         the type
     * @return Object of type which be specified
     */
    public static <T, E> T postTemplate(RestTemplate restTemplate,String url, E body, Class<T> type) {

        // 设置请求的 Content-Type 为 application/json
        MultiValueMap<String, String> header = new LinkedMultiValueMap();
        header.put(HttpHeaders.CONTENT_TYPE, Arrays.asList(MediaType.APPLICATION_JSON_VALUE));
        // 设置 Accept 向服务器表明客户端可处理的内容类型
        header.put(HttpHeaders.ACCEPT, Arrays.asList(MediaType.APPLICATION_JSON_VALUE));
        // 直接将实体 Product 作为请求参数传入，底层利用 Jackson 框架序列化成 JSON 串发送请求
        HttpEntity<E> request = new HttpEntity(body, header);

        ResponseEntity<T> exchangeResult = restTemplate.exchange(url, HttpMethod.POST, request, type);
        Assert.isTrue(exchangeResult.getStatusCode().equals(HttpStatus.OK), "请求失败");
        return exchangeResult.getBody();
    }

    /**
     * Gets for object.
     *
     * @param <T>          the type parameter
     * @param restTemplate the rest template
     * @param baseUrl      the base url
     * @param responseType the response type
     * @param uriVariables the uri variables
     * @return the for object
     */
    public static <T> T getForObject(RestTemplate restTemplate, String baseUrl, Class<T> responseType, Map<String, Object> uriVariables) {
        Assert.notNull(restTemplate, "Failed to acquire the RestTemplate bean when post");
        String fullUrl = constructGetUrl(baseUrl, uriVariables);
        log.info("full url {}", fullUrl);
        log.info("uriVariables {}", uriVariables.toString());
        T result = restTemplate.getForObject(fullUrl, responseType, uriVariables);
        return result;
    }

    /**
     * Construct the full url append variables
     *
     * @param baseUrl      the base url
     * @param uriVariables the uri variables
     * @return string
     */
    public static String constructGetUrl(String baseUrl, Map<String, ?> uriVariables) {
        if (uriVariables == null || uriVariables.isEmpty()) {
            return baseUrl;
        }
        StringBuilder builder = new StringBuilder(baseUrl);
        builder.append("?");
        uriVariables.forEach(
                (key, value) -> builder.append(key).append("={").append(key).append("}").append("&")
        );
        //remove the last '&'
        builder.deleteCharAt(builder.length() - 1);
        return builder.toString();
    }
}

```
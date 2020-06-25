### a tool about jackson utils like JSON

```java
package com.example.demo.jackson;

/**
 * @Author fei
 */

import com.fasterxml.jackson.annotation.JsonInclude.Include;
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.DeserializationFeature;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.type.TypeFactory;

import java.io.StringWriter;
import java.lang.reflect.Type;

/**
 */
public class JsonUtils {

    private static ObjectMapper objectMapper = new ObjectMapper();

    static {
        objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
        objectMapper.setSerializationInclusion(Include.NON_NULL);
    }

    private JsonUtils() {
    }

    public static String toJSONString(Object obj) {
        try {
            StringWriter writer = new StringWriter();
            objectMapper.writeValue(writer, obj);
            return writer.toString();
        } catch (Exception e) {
            throw new RuntimeException("Deserialize from JSON failed.", e);
        }
    }

    @Deprecated
    public static <T> T parseObject(String json, Class<T> clazz) {
        try {
            return objectMapper.readValue(json, clazz);
        } catch (Exception e) {
            throw new RuntimeException("Deserialize from JSON failed.", e);
        }
    }

    public static <T> T parseObject(String json, TypeReference<T> typeReference) {
        try {

            return objectMapper.readValue(json, typeReference);
        } catch (Exception e) {
            throw new RuntimeException("Deserialize from JSON failed.", e);
        }
    }

    public static <T> T parseObject(String json, Type genericType) {
        try {
            return objectMapper.readValue(json, TypeFactory.defaultInstance().constructType(genericType));
        } catch (Exception e) {
            throw new RuntimeException("Deserialize from JSON failed.", e);
        }
    }

}

```


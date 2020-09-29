### 常用日期转换

```java
import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.StringUtils;

import java.sql.Timestamp;
import java.text.SimpleDateFormat;
import java.time.Instant;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;
import java.util.Calendar;
import java.util.Date;
import java.util.GregorianCalendar;
import java.util.TimeZone;
public class TimeUtils {

    /**
     * The constant DEFAULT_PATTERN.
     */
    public static final String DEFAULT_PATTERN = "yyyy-MM-dd HH:mm:ss";

    /**
     * The constant DEFAULT_TIME_ZONE.
     */
    public static final String DEFAULT_TIME_ZONE = "GMT+00:00";

    /**
     * The constant TSDB_TIME_FORMAT.
     */
    public static final String TSDB_TIME_FORMAT ="yyyy-MM-dd'T'HH:mm:ssXXX";

    /**
     * Time to long long.
     *
     * @param timeString the time string
     * @param pattern    the pattern
     * @param timeZone   the time zone
     * @return long long
     */
    public static Long timeToMilli(String timeString, String pattern, String timeZone) {
        DateTimeFormatter ftf = DateTimeFormatter.ofPattern(pattern);
        LocalDateTime parse = LocalDateTime.parse(timeString, ftf);
        ZonedDateTime zonedDateTime = LocalDateTime.from(parse).atZone(ZoneId.of(timeZone));
        return zonedDateTime.toInstant().toEpochMilli();
    }

    /**
     * Time to long long.
     *
     * @param timeString the time string
     * @return the long
     */
    public static Long timeToMilli(String timeString) {
        return timeToMilli(timeString, DEFAULT_PATTERN, DEFAULT_TIME_ZONE);
    }

    /**
     * 将Long类型的时间戳转换成String 类型的时间格式，时间格式为：yyyy-MM-dd HH:mm:ss
     *
     * @param timeSeconds the time
     * @return the string
     */
    public static String secondTimeToString(long timeSeconds){
       return secondTimeToString(timeSeconds,DEFAULT_PATTERN,DEFAULT_TIME_ZONE);
    }

    /**
     * Time to string string.
     *
     * @param timeSeconds the time
     * @param pattern     the pattern
     * @param zoneId      the zone id
     * @return the string
     */
    public static String secondTimeToString(long timeSeconds, String pattern, String zoneId) {
        DateTimeFormatter ftf = DateTimeFormatter.ofPattern(pattern);
        String timeString = ftf.format(LocalDateTime.ofInstant(Instant.ofEpochSecond(timeSeconds), ZoneId.of(zoneId)));
        return timeString;
    }

    /**
     * Mili time to string string.
     *
     * @param timeMilli the time mili
     * @param pattern  the pattern
     * @param zoneId   the zone id
     * @return the string
     */
    public static String milliTimeToString(long timeMilli, String pattern, String zoneId) {

        Instant instant = Instant.ofEpochMilli(timeMilli);
        ZonedDateTime zonedDateTime = instant.atZone(ZoneId.of(zoneId));
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern(pattern);
        String timeString = formatter.format(zonedDateTime);

        return timeString;
    }
}

```
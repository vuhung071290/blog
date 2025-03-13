+++
date = "2020-01-29"
title = "Java Date Time API"
description = "Java Date Time API"
slug = "handling-timezone-java"
tags = ["miscellaneous", "timezone", "java", "datetime"]
categories = []
series = ["timezone"]
disable_comments = true
featuredImage = "/images/handling-timezone-java/Java-8-Date-Time-API.jpg"
+++

Trước Java 8, các nhà phát triển sử dụng java.util.Date, java.util.Calendar và java.text.SimpleDateFormat cho các thao tác liên quan đến ngày giờ. Tuy nhiên, các lớp này có những vấn đề lớn khiến chúng khó sử dụng.

## Thách thức với các lớp Calendar và Date
+ **Không thân thiện với người dùng**: Cả lớp Date và Calendar đều khó hiểu và sử dụng. Ví dụ, tháng trong Date và Calendar được đánh số từ 0, điều này gây khó hiểu (tháng Một là 0, không phải 1).
+ **Vấn đề Xử lý múi giờ**: Xử lý múi giờ gặp nhiều khó khăn với cả Date và Calendar. Date đại diện [epoch](https://en.wikipedia.org/wiki/Unix_time) nhưng khi hiển thị sẽ chuyển đổi sang múi giờ hệ thống, dễ gây nhầm lẫn.
+ **Không an toàn trong đa luồng**: Cả Date, Calendar và SimpleDateFormat đều có thể thay đổi được nên không an toàn trong môi trường đa luồng
+ **Mã dư thừa**: Thực hiện các thao tác đơn giản như tính toán ngày thường yêu cầu phải viết nhiều mã, gây rườm rà và dễ dẫn đến lỗi.

Những vấn đề này là lý do khiến các thư viện ngày và giờ của bên thứ ba như [Joda-Time](https://www.joda.org/joda-time/) trở nên phổ biến.

## Giới thiệu về API Date-Time trong Java 8

Lấy cảm hứng từ Joda-Time, API DateTime mới của Java 8 được thiết kế để giải quyết những vấn đề này, cung cấp một giải pháp toàn diện, trực quan và đáng tin cậy hơn cho việc xử lý ngày giờ.

{{< figure src="/images/handling-timezone-java/Java_Datetime.png" caption="">}}

Diagram tóm tắt cách chuyển đổi qua lại giữa các class

{{< figure src="/images/handling-timezone-java/Java_Datetime_Diagram.png" caption="">}}

Chi tiết về cách sử dụng các lớp này trong Java

### LocalDate (Chỉ đại diện cho ngày)
LocalDate chỉ lưu trữ thông tin về ngày (năm, tháng, ngày) mà không có thông tin giờ.

Khởi tạo:
```java
LocalDate date1 = LocalDate.now(); // Lấy ngày hiện tại
LocalDate date2 = LocalDate.of(2025, 3, 13); // Tạo ngày cụ thể
```

Các phương thức quan trọng:
```java
LocalDate today = LocalDate.now();
LocalDate tomorrow = today.plusDays(1); // Cộng thêm 1 ngày
LocalDate nextMonth = today.plusMonths(1); // Cộng thêm 1 tháng
LocalDate previousYear = today.minusYears(1); // Trừ đi 1 năm

System.out.println(today.getDayOfWeek()); // Lấy ngày trong tuần (MONDAY, TUESDAY,...)
```

### LocalTime (Chỉ đại diện cho giờ)
LocalTime lưu trữ thông tin về thời gian (giờ, phút, giây) mà không có thông tin ngày.

Khởi tạo:
```java
LocalTime time1 = LocalTime.now(); // Lấy thời gian hiện tại
LocalTime time2 = LocalTime.of(15, 30); // Tạo thời gian cụ thể: 15:30
```

Các phương thức quan trọng:
```java
LocalTime now = LocalTime.now();
LocalTime after1Hour = now.plusHours(1); // Cộng thêm 1 giờ
LocalTime before30Minutes = now.minusMinutes(30); // Trừ đi 30 phút

System.out.println(now.getHour()); // Lấy giờ
System.out.println(now.getMinute()); // Lấy phút
```

### LocalDateTime (Đại diện cho cả ngày và giờ)
LocalDateTime kết hợp cả ngày và giờ nhưng không có múi giờ.

Khởi tạo:
```java
LocalDateTime dateTime1 = LocalDateTime.now(); // Lấy ngày giờ hiện tại
LocalDateTime dateTime2 = LocalDateTime.of(2025, 3, 13, 10, 30); // Tạo ngày giờ cụ thể
```

Các phương thức quan trọng:
```java
LocalDateTime now = LocalDateTime.now();
LocalDateTime tomorrow = now.plusDays(1); // Cộng thêm 1 ngày
LocalDateTime nextHour = now.plusHours(1); // Cộng thêm 1 giờ
LocalDateTime previousMinute = now.minusMinutes(1); // Trừ đi 1 phút

System.out.println(now.getYear()); // Lấy năm
System.out.println(now.getMonthValue()); // Lấy tháng
```

### ZonedDateTime (Đại diện cho ngày giờ với múi giờ)
ZonedDateTime đại diện cho ngày giờ với múi giờ và khu vực địa lý (zone). Mỗi ZonedDateTime chứa thông tin về múi giờ và tên khu vực (như "Europe/Paris" hay "America/New_York"). Đây là lớp quan trọng khi cần xử lý múi giờ trong ứng dụng.

Khởi tạo:
```java
ZonedDateTime zonedDateTime1 = ZonedDateTime.now(); // Lấy ngày giờ hiện tại với múi giờ
ZonedDateTime zonedDateTime2 = ZonedDateTime.of(2025, 3, 13, 10, 30, 0, 0, ZoneId.of("Asia/Ho_Chi_Minh")); // Tạo ngày giờ cụ thể với múi giờ
```

Các phương thức quan trọng:
```java
ZonedDateTime now = ZonedDateTime.now();
ZonedDateTime newYorkTime = now.withZoneSameInstant(ZoneId.of("America/New_York")); // Chuyển sang múi giờ New York
ZonedDateTime plus5Hours = now.plusHours(5); // Cộng thêm 5 giờ

System.out.println(now.getZone()); // Lấy múi giờ
```

Sự khác biệt giữa ZonedDateTime và OffsetDateTime:
> ZonedDateTime: Dùng khi bạn cần thông tin chi tiết về múi giờ và tên khu vực, đặc biệt là khi cần xử lý các quy tắc thay đổi giờ mùa hè hay khi làm việc với nhiều khu vực khác nhau trên thế giới.
> OffsetDateTime: Dùng khi bạn chỉ quan tâm đến sự chênh lệch giữa giờ UTC và giờ địa phương mà không cần biết khu vực cụ thể.


### Instant (Đại diện cho thời gian tuyệt đối)
Instant đại diện cho thời gian tính từ thời điểm "Epoch" (01/01/1970). Lớp này chủ yếu dùng để làm việc với thời gian tuyệt đối hoặc timestamps.

Khởi tạo:
```java
Instant instant1 = Instant.now(); // Lấy thời gian hiện tại
Instant instant2 = Instant.ofEpochSecond(3600); // Tạo thời gian từ giây (tính từ Epoch)
```

Các phương thức quan trọng:
```java
Instant now = Instant.now();
Instant later = now.plusSeconds(3600); // Cộng thêm 1 giờ (3600 giây)
Instant before = now.minusMillis(5000); // Trừ đi 5000 mili giây

System.out.println(now.toString()); // Chuyển thành chuỗi ISO-8601
```

### DateTimeFormatter (Định dạng thời gian)
DateTimeFormatter giúp chuyển đổi các đối tượng LocalDate, LocalTime, LocalDateTime thành chuỗi định dạng và ngược lại.

Khởi tạo:
```java
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
```

Định dạng và phân tích:
```java
LocalDateTime now = LocalDateTime.now();
String formatted = now.format(formatter); // Định dạng thành chuỗi
System.out.println(formatted);

LocalDateTime parsed = LocalDateTime.parse("2025-03-13 10:30:00", formatter); // Phân tích chuỗi thành LocalDateTime
System.out.println(parsed);
```

## Tóm tắt:
Java 8 đã cung cấp một API mạnh mẽ và linh hoạt để xử lý thời gian và ngày tháng. Sử dụng các lớp như LocalDate, LocalTime, LocalDateTime, ZonedDateTime, và Instant giúp bạn xử lý các tình huống liên quan đến thời gian trong ứng dụng một cách dễ dàng và chính xác hơn.


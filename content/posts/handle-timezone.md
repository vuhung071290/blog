+++
date = "2020-01-25"
title = "Kinh nghiệm làm việc với DateTime"
description = "Kinh nghiệm làm việc với DateTime"
slug = "handling-timezone"
tags = ["miscellaneous", "timezone"]
categories = []
series = ["timezone"]
disable_comments = true
featuredImage = "/images/handling-timezone/timezones-for-developers.png"
+++

Trong lập trình chúng ta phải làm việc với **DateTime** khá nhiều, trong đó **TimeZone** cũng là một vấn đề nan giải, bài viết này là tổng hợp những kinh nghiệm khi làm việc với kiểu dữ liệu này.

## Qui chuẩn quốc tế khi biểu diễn DateTime

Để biểu diễn ngày giờ theo [hệ thống giờ quốc tế](https://vuhung071290.github.io/posts/timezone/) ta có các qui chuẩn sau:

+ [Epoch time](https://en.wikipedia.org/wiki/Unix_time) là biểu diễn dưới dạng số, đo bằng số giây không nhuận đã trôi qua kể từ 00:00:00 UTC ngày 1 tháng 1 năm 1970, ví dụ
  > 1623606902

+ [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601) là biểu diễn dưới dạng text, ví dụ
  + Biểu diễn giờ UTC 
  > 2021-01-17T17:18:46Z
  + Biểu diễn giờ địa phương với UTC offset
  > 2021-01-17T19:18:46+02:00

[IANA Time Zone Database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) Hay còn gọi là tz database, là một bộ database tổng hợp thông tin của toàn bộ múi giờ trên thế giới, được quản lý bởi tổ chức ICANN (Nếu bạn chưa biết thì múi giờ có thể thay đổi vì địa chính trị, hoặc theo mùa do áp dụng giờ [DST](https://vuhung071290.github.io/posts/timezone/#32-gi%e1%bb%9d-dst-daylight-saving-time)).
Trong tz database, một [múi giờ](https://vuhung071290.github.io/posts/timezone/#3-timezone) sẽ có tên gọi dựa trên vị trí địa lý của nó, theo dạng Area/Location, trong đó area là tên của lục địa hoặc đại dương, location là tên của thành phố hoặc hòn đảo.

> Múi giờ ở thành phố Hồ Chí Minh có tên là Asia/Ho_Chi_Minh
Múi giờ ở Auckland (New Zealand) có tên là Pacific/Auckland
Tuy nhiên, không phải thành phố nào cũng có múi giờ riêng. Tham khảo danh sách đầy đủ [ở đây](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

## Các nguyên tắc khi xử lý DateTime

## Các thư viện DateTime
### Java DateTime
### Mysql Datetime


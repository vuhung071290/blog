+++
date = "2020-01-27"
title = "Kinh nghiệm làm việc với datetime"
description = "Kinh nghiệm làm việc với datetime"
slug = "handling-timezone"
tags = ["miscellaneous", "timezone"]
categories = []
series = ["timezone"]
disable_comments = true
featuredImage = "/images/handling-timezone/timezones-for-developers.png"
+++

Việc xử lý [thời gian](https://vuhung071290.github.io/posts/timezone/) đã là một chủ đề mang lại nhiều cơn đau đầu cho các developers, đặc biệt nếu phải xử lý thời gian theo nhiều [múi giờ](https://vuhung071290.github.io/posts/timezone/#timezone-m%c3%bai-gi%e1%bb%9d) khác nhau, bài viết này là tổng hợp một số kinh nghiệm để xử lý kiểu dữ liệu này tốt hơn.

## Một số thuật ngữ
+ **moment**: thời gian tuyệt đối
+ **rtime** (relative/represent time): thời gian tương đối, hoặc cũng có thể gọi là thời gian chỉ để hiển thị
+ **timezone**: [múi giờ](https://vuhung071290.github.io/posts/timezone/#timezone-m%c3%bai-gi%e1%bb%9d)
+ **UTC offset**: độ lệch so với [giờ chuẩn UTC](https://vuhung071290.github.io/posts/timezone/#gi%e1%bb%9d-utc-coordinated-universal-time)

> moment = rtime + timezone (timezone được đặc trưng bởi UTC offset)

## Qui chuẩn quốc tế khi biểu diễn datetime

+ [Epoch time](https://en.wikipedia.org/wiki/Unix_time) là biểu diễn dưới dạng số, đo bằng số giây không nhuận đã trôi qua kể từ 00:00:00 UTC ngày 1 tháng 1 năm 1970
  > 1623606902

+ [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) là biểu diễn dưới dạng text
  + Biểu diễn giờ UTC 
  > 2021-01-17T17:18:46Z
  + Biểu diễn giờ địa phương với UTC offset
  > 2021-01-17T19:18:46+02:00
  
+ [IANA Time Zone Database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) còn gọi là tz database, được quản lý bởi tổ chức ICANN. Đây là một bộ database tổng hợp thông tin của toàn bộ múi giờ trên thế giới (vì múi giờ có thể thay đổi do kinh tế, chính trị, [giờ DST](https://vuhung071290.github.io/posts/timezone/#gi%e1%bb%9d-dst-daylight-saving-time)). 

## Các nguyên tắc khi xử lý datetime
+ **Rule 1: Store datetime in UTC in your database and back end code**: Nếu dữ liệu của bạn lưu trữ ở những múi giờ khác nhau, thì việc thống nhất khi lưu và xử lý data sẽ tránh được lỗi về sai lệch múi giờ.
+ **Rule 2: Convert datetime to the user's local timezone using frontend code**: Mặc dù backend trả về thời gian UTC, nhưng frontend có thể dễ dàng chuyển đổi chúng sang múi giờ địa phương của người dùng. Việc này tạo ra sự phân chia nhiệm vụ giữa backend (xử lý theo UTC) và frontend (xử lý theo thời gian địa phương của người dùng). Hãy giữ cho định dạng thời gian trong frontend nhất quán bằng cách sử dụng một tiêu chuẩn, chẳng hạn như ISO 8601. Khi gửi yêu cầu đến backend, hãy gửi thời gian ở định dạng ISO 8601 để backend có thể dễ dàng chuyển đổi nó thành thời gian UTC tương ứng.
+ **Rule 3: Use datetime libraries**: Hãy luôn sử dụng những thư viện trong các ngôn ngữ/framework để xử lý datetime tốt hơn. Chúng giúp việc chuyển đổi hoặc định dạng theo các tiêu chuẩn (ví dụ: Epoch time, ISO 8601) trở nên dễ dàng hơn nhiều.

## Các thư viện datetime
### Java datetime

### Mysql Datetime


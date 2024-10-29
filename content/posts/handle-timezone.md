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

Trong lập trình chúng ta phải làm việc với **DateTime** khá nhiều, trong đó [TimeZone](https://vuhung071290.github.io/posts/timezone/#timezone-m%c3%bai-gi%e1%bb%9d) cũng là một vấn đề dễ gây nhầm lẫn, bài viết này là tổng hợp những kinh nghiệm khi làm việc với kiểu dữ liệu này.

## Qui chuẩn quốc tế khi biểu diễn DateTime

Để biểu diễn DateTime theo [giờ quốc tế](https://vuhung071290.github.io/posts/timezone/) ta có các qui chuẩn sau:

+ [Epoch time](https://en.wikipedia.org/wiki/Unix_time) là biểu diễn dưới dạng số, đo bằng số giây không nhuận đã trôi qua kể từ 00:00:00 UTC ngày 1 tháng 1 năm 1970, ví dụ
  > 1623606902

+ [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601) là biểu diễn dưới dạng text, ví dụ
  + Biểu diễn giờ UTC 
  > 2021-01-17T17:18:46Z
  + Biểu diễn giờ địa phương với UTC offset
  > 2021-01-17T19:18:46+02:00
  > 
Ngoài ra, múi giờ có thể thay đổi do địa chính trị, hoặc do áp dụng [giờ DST](https://vuhung071290.github.io/posts/timezone/#gi%e1%bb%9d-dst-daylight-saving-time) nên cũng cần có [IANA Time Zone Database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones), còn gọi là tz database.
Đây là một bộ database tổng hợp thông tin của toàn bộ múi giờ trên thế giới, được quản lý bởi tổ chức ICANN. Trong tz database, một múi giờ sẽ có tên gọi dựa trên vị trí địa lý của nó, theo dạng Area/Location, trong đó area là tên của lục địa hoặc đại dương, location là tên của thành phố hoặc hòn đảo. Tuy nhiên, không phải thành phố nào cũng có múi giờ riêng.
> Múi giờ ở thành phố Hồ Chí Minh có tên là Asia/Ho_Chi_Minh
Múi giờ ở Auckland (New Zealand) có tên là Pacific/Auckland

## Các nguyên tắc khi xử lý DateTime
Việc tuân thủ những qui tắc sau giúp chúng ta bớt đau đầu trong việc xử lý timezone
+ **Rule 1: Store datetime in UTC in your database and back end code**: Hãy tưởng tượng dữ liệu của bạn lưu trữ ở những timezone khác nhau, thì việc thống nhất lưu data ở múi giờ nào rất quan trọng, lưu giờ UTC là lựa chọn hợp lý nhất.
+ **Rule 2: Convert datetime to the user's local timezone using frontend code**:  Mặc dù backend của bạn sẽ trả về thời gian UTC, nhưng frontend có thể dễ dàng chuyển đổi chúng sang múi giờ địa phương của người dùng. Việc này tạo ra sự phân chia nhiệm vụ giữa backend (xử lý theo UTC) và frontend (xử lý theo thời gian địa phương của người dùng). Hãy giữ cho định dạng thời gian trong frontend nhất quán bằng cách sử dụng một tiêu chuẩn, chẳng hạn như ISO 8601. Khi bạn gửi yêu cầu đến backend, hãy gửi thời gian ở định dạng ISO 8601 để backend có thể dễ dàng chuyển đổi nó thành thời gian UTC tương ứng.
+ **Rule 3: Use datetime libraries**: Hãy luôn sử dụng những thư viện trong các ngôn ngữ/framework để xử lý datetime tốt hơn. Chúng giúp việc chuyển đổi hoặc định dạng theo các tiêu chuẩn (ví dụ: Epoch time, ISO 8601) trở nên dễ dàng hơn nhiều, ngoài ra chúng còn hỗ trợ xử lý giờ DST.

## Các thư viện DateTime
Ở **Rule 3** chúng ta nói đề cập về việc sử dụng các thư viện, nên trong phần này chúng ta sẽ tìm hiểu một số thư viện để hiểu hơn về cách lưu trữ, xử lý datetime của chúng
### Java DateTime
### Mysql Datetime


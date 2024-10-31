+++
date = "2020-01-29"
title = "Java Date Time API"
description = "Java Date Time API"
slug = "handling-timezone-java"
tags = ["miscellaneous", "timezone", "java", "datetime"]
categories = []
series = ["timezone"]
disable_comments = true
featuredImage = "/images/handling-timezone-java/Java_Datetime.png"
+++

Khi xử lý thời gian, trước đây chúng ta thường dùng Date, SimpleDateFormat và Calendar. Date là một moment, thể hiện ngày giờ với TimeZone mặc định của server. Tuy nhiên do thiếu sót trong thiết kế ban đầu nên phần lớn method đã bị deprecated và thay thế bởi Instant

Kể từ phiên bản 1.8 trở đi, Java cung cấp những API mạnh mẽ để xử lý ngày - giờ. Dưới đây là diagram tóm tắt cách chuyển đổi qua lại giữa các class

{{< figure src="/images/handling-timezone-java/Java_Datetime_Diagram.png" caption="">}}


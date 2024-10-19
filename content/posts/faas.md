+++
date = "2019-06-24"
title = "Serverless, xu hướng mới của điện toán đám mây"
description = "Serverless, xu hướng mới của điện toán đám mây."
slug = "faas"
tags = ["cloud", "serverless", "faas"]
categories = []
series = ["serverless"]
disable_comments = true
featuredImage = "/images/faas/faas1.jpg"
+++

## Serverless, Faas là gì

Serverless là nền tảng thực thi ứng dụng mà trong đó việc phân bổ quản lý tài nguyên sẽ do nền tảng này đảm nhiệm, chúng ta chỉ cần tập trung vào việc phát triển ứng dụng của mình.

{{< figure src="/images/faas/faas2.jpg" caption="">}}

Serverless có hai dịch vụ chính:

+ Backend as a Service (Baas)

    Một số ứng dụng chuyển phần lớn logic về front-end, không có server để làm back-end mà chỉ sử dụng các API của bên thứ 3 để thay thế, có rất nhiều đơn vị thứ 3 cung cấp dịch vụ này như Parse, Firebase, AWS Cognito, ... Cách này khá tiện lợi vì giảm được việc viết code phần backend nhưng có nhược điểm là phụ thuộc nhiều vào bên thứ 3

+ Function as Service (FaaS)

    Ở mô hình này, bạn sẽ phải viết code ở phần backend, nhưng khi deploy lên server, bạn deploy dưới dạng một Function, Funtion này có thể được gọi từ RestAPI hoặc chạy theo lịch đã sắp sẵn.

    Hiện tại, khi nói đến serverless, người ta thường nói đến khái niệm thứ hai – FaaS.

## Ưu nhược điểm của Serverless

{{< figure src="/images/faas/faas3.jpg" caption="" >}}

### Ưu điểm

+ Giá cả phải chăng: Với kiến trúc server truyền thống, bạn phải thuê server theo tháng. Với kiến trúc Serverless, do chi phí được tính theo số lần gọi Function nên rất rẻ.

+ Dễ scale mở rộng: Khi số lượng request tăng cao, bạn phải nâng cấp server để đảm bảo tốc độ, việc này khá rườm rà mất thời gian. Với Serverless, bên thứ 3 sẽ tự động tạo thêm nhiều process khi có nhiều request.

+ Đơn giản hoá việc code và deploy: Với server truyền thống, bạn phải biết cách build, deploy code lên server, bảo trì và kết nối tới server. Với Serverless, bạn chỉ việc code, mọi việc còn lại sẽ được thực hiện giúp bạn.

### Nhược điểm

+ Bảo mật: Để sử dụng tài nguyên hiệu quả, nhà cung cấp dịch vụ có thể cài đặt ứng dụng của nhiều khách hàng khác nhau trên cùng một server vật lý. Điều đó tiềm ẩn nguy cơ về bảo mật, nếu như ứng dụng của bạn làm việc với dữ liệu quan trọng.

+ Performance: Serverless không hoạt động hiệu quả đối với ứng dụng dạng long-running. Một request trên serverless có thể tốn nhiều thời gian hơn vì sẽ mất khoảng 20-50ms để warn-up.

+ Phụ thuộc vào nhà cung cấp dịch vụ: Khó debug, tài nguyên (CPU, RAM) được bên thứ 3 quản lý nên đôi khi ta sẽ khó mà tái tạo môi trường ở local để debug code của FaaS.

## Cách họat động của Faas

Như đã đề cập với Faas chúng ta deploy các function, các function này hoạt động theo kiến trúc event-driven, mỗi function được định nghĩa các event-source (một request REST, một file được upload, một record mới được insert trong database...) khi các event-source được kích hoạt, server sẽ thực thi code của function tương ứng.

{{< figure src="/images/faas/faas4.jpg" caption="" >}}

Hiện nay ngoài các nhà cung cấp dịch vụ Faas lớn như AWS Lambda, Azure Functions hay Google functions, còn có nhiều các [framework, tools open source](https://github.com/anaibol/awesome-serverless) đã và đang được phát triển dành cho Faas, nổi bật là các framework được phát triển dựa trên nền tảng của k8s như openFaas, openWhisk, Fn, Kubeless, ...

Để hiểu rõ cách một hệ thống serverless hoạt động, hãy xem qua kiến trúc của openFaas

{{< figure src="/images/faas/faas5.jpg" caption="" >}}

openFaas gồm hai thành phần chính:

{{< figure src="/images/faas/faas6.png" caption="API Gateway: chịu trách nhiệm đón các request được gọi đến hệ thống, forward các request này đến watchdog, ngoài ra nó còn collect các metrics của functions để thực hiện monitoring" >}}

{{< figure src="/images/faas/faas7.jpg" caption="Watchdog: chịu trách nhiệm khởi chạy và quản lý các functions" >}}

Các bước để  deploy một function trong OpenFaas:

1. Tạo một function rồi đăng kí với OpenFaas qua UI hoặc CLI dưới dạng docker image, image này sẽ được Watchdog quản lý và dùng để khởi tạo function

2. Khi một request REST được gọi đến hệ thống, API Gateway forward các request này đến watchdog, dựa trên số lượng request API Gateway cũng gửi kèm các thông số về số lượng để Watchdog scale số pod/container

3. Sau khi nhận request, Watchdog làm 3 việc
   + đầu tiên nó tạo các biến môi trường từ header của request
   + sau đó khởi chạy function bằng docker swarm với image function đã được đăng kí trước đó
   + cuối cùng nó truyền nội dụng của request vào function đã tạo thông qua stdin

4. Sau khi function thực hiện xong, Watchdog trả kết quả về thông qua stdout, sau đó trả về cho user thông qua API Gateway

    [Demo Hello world fuction với OpenFaas chạy trên minikube](https://medium.com/@lizrice/getting-started-with-openfaas-on-minikube-8d51987f5bbb)

## Kết

+ Serverless không phải là giải pháp thay thế cho server long-running truyền thống, với ưu nhược điểm của mình serverless phù hợp cho một số [usecases](https://www.simform.com/serverless-examples-aws-lambda-use-cases/) nhất định. trong thực tế các ứng dụng phức tạp có thể sử dụng các server truyền thống kết hợp với functions serverless.

+ Vì serverless còn rất mới mẻ nên chưa có một chuẩn chung về cách vận hành và sử dụng giữa các nhà cung cấp, việc thiếu vắng các tool hỗ trợ về debug, monitoring, các framework chưa mature cũng là trở ngại lớn.

+ Hiện tại, Serverless là một xu hướng phù hợp cho những công ty đang sử dụng cloud của các ông lớn để tiết kiệm chi phí vận hành và thời gian code, với những công ty sử dụng cơ sở hạ tầng riêng hoặc private cloud có lẽ nên chờ thêm một gian để hệ sinh thái open source của serverless hoàn thiện hơn.
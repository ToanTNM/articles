# Các khái niệm về Captive portal

- Captive portal: trang để người dùng thực hiện xác thực khi truy cập mạng

- Captive portal được kích hoặc từ client bằng 2 cách:  
  - DNS redirection: chiếm quyền DNS, tất cả các request từ client sẽ được chuyển đến trang đăng nhập. Tuy nhiên, khi HSTS được sử dụng rộng rãi thì tỉ lệ đăng nhập thành công của cách này thấp, làm giảm trải nghiệm của người dùng. Người dùng phải kích hoạt trang đăng nhập bằng tay.
  - Splash page: cũng sử dụng DNS redirection, nó nhận phản hồi lại các request và hệ điều hành tự động kích hoạt trang đăng nhập. Giúp vượt qua cơ chế HSTS ở hầu hết các trang web, đồng thời hiển thị trang đăng nhập một cách linh hoạt và tự động.

# Cơ chế phát hiện (detection)

- Mỗi OS có cách riêng để phát hiện việc truy cập Internet. Cơ chế cơ bản:
  
```JS
GET/POST http://foo.com/bar.html
If bar.html == [expected content] > Open Internet
If bar.html != [expected content] > Captive Portal
If bar.html[status] != SUCCESS > No Network
```

- Nếu không cấu hình Captive Portal -> nội dung html OS nhận được đúng như mong muốn -> kết nối Internet OK
- Nếu có Captive Portal -> nội dung html nhận được khác với mong muốn -> OS tự động mở splash page
- URL để detect có thể khác nhau tùy thuộc vào OS, model thiết bị:
  - ```Android``` 4-9: detect response HTTP 204 (File exist, but empty), file generated_204 từ các domain:

    ```url
    clients3.google.com
    connectivitycheck.android.com
    connectivitycheck.gstatic.com
    ```

    Nếu nhận được HTTP 302 (temporary redirect) thì ```Android``` sẽ chuyển hướng đến Captive portal.
    Như vậy, tất cả các server hoạt động với giao thức HTTP mà không có HTTPS sẽ luôn có thể kích hoạt cơ chế Captive portal, cho phép attacker vượt qua bảo mật.

  - ```Windows```
  
    ```url
    www.msftconnecttest.com
    www.msftncsi.com
    ```

    - ```Apple``` ```iOS 7+``` và ```MacOS 10.10+```

    ```url
    captive.apple.com/hotspot-detect.html
    www.apple.com/library/test/success.html
    ```

    Các thiết bị của ```Apple``` sử dụng WISPr (đọc là "whisper") - Wireless Internet Service Provider roaming để phát hiện Captive Portal. Công nghệ này cho phép người dùng chuyển vùng giữa các dịch vụ Internet không dây giống như người dùng di động.

    Các Captive Portal sẽ phải phát hiện các thiết bị hỗ trợ WISPr và trả về các bản tin WISPr dưới dạng XML.

    Điều này cho phép các thiết bị được cache chứng thực sau khi đăng nhập thành công qua bản tin XML khác. Nghĩa là có thể lấy xác thực của người khác để sử dụng mạng bình thường.

    Từ ```iOS 7```, ```Apple``` sử dụng User Agent "CaptiveNetworkSupport". Lý do là người dùng có thể sử dụng các app hay trình duyệt khác nhau 

# Tham khảo thêm

[CAPTIVE PORTAL: The Definitive Guide](https://rootsh3ll.com/captive-portal-guide/)

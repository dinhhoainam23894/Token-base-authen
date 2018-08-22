# The Ins and Outs of Token Based Authentication

## Giới thiệu

Xác thực dựa trên token luôn nổi bật ở khắp mọi nơi trên web hiện nay. Với hầu hết mọi công ty web sử dụng API, Token là cách tốt nhất để xử lý xác thực cho nhiều người dùng.

Có một số yếu tố rất quan trọng khi chọn xác thực dựa trên token cho ứng dụng của bạn. Lý do chính cho việc sử dụng token là:

Máy chủ không trạng thái và có thể mở rộng
Khả dụng đối với ứng dụng di động
Chuyển xác thực cho các ứng dụng khác
Tăng cường bảo mật

## Ai sử dụng xác thực dựa trên token ?

Bất kỳ API hoặc ứng dụng web chính nào mà bạn đã gặp phải đều có khả năng sử dụng token nhất. Các ứng dụng như Facebook, Twitter, Google+, GitHub và rất nhiều cái khác.

Chúng ta hãy xem chính xác cách thức hoạt động của nó.

## Why Tokens Came Around
Trước khi chúng ta có thể thấy cách xác thực dựa trên token hoạt động và lợi ích của nó, chúng ta phải xem xét cách xác thực đã được thực hiện trong quá khứ.

### Xác thực dựa trên máy chủ (Phương thức truyền thống)

Vì giao thức HTTP là không trạng thái, điều này có nghĩa là nếu chúng ta xác thực người dùng bằng tên người dùng và mật khẩu, thì ở yêu cầu tiếp theo, ứng dụng của chúng ta sẽ không biết chúng ta là ai. Chúng ta sẽ phải xác thực lại.

Cách truyền thống để ứng dụng của chúng ta nhớ chúng ta là ai để lưu trữ người dùng đăng nhập thông tin trên máy chủ. Điều này có thể được thực hiện theo một vài cách khác nhau trong phiên, thường là trong bộ nhớ hoặc được lưu trữ trên đĩa.

Khi web, ứng dụng và sự nổi lên của ứng dụng di động xuất hiện, phương pháp xác thực này đã cho thấy các vấn đề, đặc biệt là trong khả năng mở rộng.

## Các vấn đề với xác thực dựa trên máy chủ

Một số vấn đề lớn nảy sinh với phương pháp xác thực này.

Phiên: Mỗi lần người dùng được xác thực, máy chủ sẽ cần phải tạo bản ghi ở đâu đó trên máy chủ của chúng ta. Điều này thường được thực hiện trong bộ nhớ và khi có nhiều người dùng xác thực, chi phí trên máy chủ của bạn tăng lên.

Khả năng mở rộng: Vì các phiên được lưu trữ trong bộ nhớ, điều này tạo ra các vấn đề với khả năng mở rộng. Khi các nhà cung cấp dịch vụ đám mây của chúng ta bắt đầu sao chép các máy chủ để xử lý tải ứng dụng, có thông tin quan trọng trong bộ nhớ phiên sẽ hạn chế khả năng mở rộng của chúng ta

CORS: Vì chúng ta muốn mở rộng ứng dụng của mình để cho phép dữ liệu của chúng ta được sử dụng trên nhiều thiết bị di động, chúng ta lo lắng về việc chia sẻ tài nguyên gốc (CORS). Khi sử dụng các lệnh gọi AJAX để lấy các tài nguyên từ một miền khác (điện thoại di động đến máy chủ API của chúng ta), chúng ta có thể gặp sự cố với các yêu cầu bị cấm.

CSRF: Chúng ta cũng sẽ có sự bảo vệ chống lại yêu cầu giả mạo cross-site (CSRF). Người dùng dễ bị tấn công CSRF vì họ có thể đã được xác thực với một trang web ngân hàng và điều này có thể được tận dụng khi truy cập các trang web khác.
Với những vấn đề này, khả năng mở rộng là vấn đề chính, bạn nên thử một cách tiếp cận khác.

## Cách hoạt động dựa trên token
Xác thực dựa trên token là không trạng thái. Chúng ta không lưu trữ bất kỳ thông tin nào về người dùng của chúng ta trên máy chủ hoặc trong một phiên.

Khái niệm này tự thân sẽ chăm sóc nhiều vấn đề với việc phải lưu trữ thông tin trên máy chủ.

Không có thông tin phiên nào có nghĩa là ứng dụng của bạn có thể mở rộng quy mô và thêm nhiều máy hơn nếu cần mà không phải lo lắng về nơi người dùng đăng nhập.

Mặc dù việc triển khai này có thể thay đổi, nhưng ý chính của nó như sau:

Truy cập yêu cầu người dùng với Tên người dùng / Mật khẩu
Ứng dụng xác thực thông tin đăng nhập
Ứng dụng cung cấp token đã đăng ký cho khách hàng
Khách hàng lưu trữ token và gửi cùng với mọi yêu cầu
Máy chủ xác minh token và phản hồi dữ liệu

Mỗi yêu cầu sẽ yêu cầu token. token này sẽ được gửi trong headers HTTP để chúng ta tiếp tục với các ý tưởng về các yêu cầu HTTP không trạng thái. Chúng ta cũng sẽ cần phải đặt máy chủ của mình chấp nhận yêu cầu từ tất cả các miền sử dụng

Access-Control-Allow-Origin: *. Điều thú vị về việc chỉ định * trong tiêu đề ACAO là nó không cho phép yêu cầu cung cấp thông tin xác thực như xác thực HTTP, chứng chỉ SSL phía máy khách hoặc cookie.

Khi chúng ta đã xác thực thông tin của mình và chúng ta có token, chúng ta có thể thực hiện nhiều việc với token này.

Chúng ta thậm chí có thể tạo token theo quyền và chuyển mã này cùng với ứng dụng của bên thứ ba (nói cho một ứng dụng dành cho thiết bị di động mới mà chúng tôi muốn sử dụng) và họ sẽ có thể truy cập vào dữ liệu của chúng ta - nhưng chỉ thông tin mà chúng ta cho phép với token cụ thể đó.

## Lợi ích của token

### Không có trạng thái và có thể mở rộng

Các token được lưu trữ ở phía máy khách. Hoàn toàn không trạng thái và sẵn sàng để được thu nhỏ. Cân bằng tải của chúng ta có thể chuyển người dùng đến bất kỳ máy chủ nào của chúng ta vì không có thông tin trạng thái hoặc phiên nào ở bất kỳ đâu.

Nếu chúng ta giữ thông tin phiên về người dùng đã đăng nhập, điều này sẽ yêu cầu chúng ta tiếp tục gửi người dùng đó đến cùng một máy chủ mà họ đã đăng nhập tại đó(được gọi là mối quan hệ phiên).

Điều này mang lại vấn đề kể từ khi một số người dùng sẽ bị buộc phải cùng một máy chủ và điều này có thể mang lại một điểm lưu lượng truy cập lớn.

Đừng lo lắng! Những vấn đề này sẽ biến mất với token vì bản thân token giữ dữ liệu cho người dùng đó.

## Bảo mật

Token , không phải cookie, được gửi theo mọi yêu cầu và vì không có cookie nào được gửi, điều này giúp ngăn chặn các cuộc tấn công CSRF. Ngay cả khi triển khai cụ thể của bạn lưu trữ token trong một cookie ở phía máy khách, cookie chỉ là một cơ chế lưu trữ thay vì một cơ chế xác thực. Không có thông tin dựa trên phiên để thao tác vì chúng ta không có phiên!

Token cũng hết hạn sau một khoảng thời gian nhất định, do đó, người dùng sẽ được yêu cầu đăng nhập lại một lần nữa. Điều này giúp chúng ta luôn an toàn. Ngoài ra còn có khái niệm thu hồi token cho phép chúng ta vô hiệu hóa token cụ thể và thậm chí là một nhóm token dựa trên cùng một quyền.

## Khả năng mở rộng (Bạn của một người bạn và các quyền)
Tokens sẽ cho phép chúng ta xây dựng các ứng dụng chia sẻ quyền với nhau. Ví dụ, chúng ta đã liên kết các tài khoản xã hội ngẫu nhiên với những tài khoản chính của chúng ta như Facebook hoặc Twitter.
Khi chúng ta đăng nhập vào Twitter thông qua một dịch vụ (như bộ đệm), chúng ta đang cho phép Bộ đệm đăng lên luồng Twitter của chúng ta.

Bằng cách sử dụng token, đây là cách chúng ta cung cấp quyền chọn lọc cho các ứng dụng của bên thứ ba. Chúng ta thậm chí có thể xây dựng API của riêng mình và cung cấp các mã thông báo quyền đặc biệt nếu người dùng của chúng ta muốn cấp quyền truy cập vào dữ liệu của họ cho một ứng dụng khác.

## Đa nền tảng và tên miền

Chúng ta đã nói một chút về CORS trước đó. Khi ứng dụng và dịch vụ của chúng ta mở rộng, chúng ta sẽ cần cung cấp quyền truy cập vào tất cả các loại thiết bị và ứng dụng (vì ứng dụng của chúng ta chắc chắn sẽ trở nên phổ biến!).
Khi API của chúng ta chỉ phân phối dữ liệu, chúng ta cũng có thể đưa ra lựa chọn thiết kế để phân phát nội dung từ CDN. Điều này giúp loại bỏ các vấn đề mà CORS mang lại sau khi chúng ta thiết lập cấu hình  header nhanh cho ứng dụng của chúng ta.
Dữ liệu và tài nguyên của chúng ta sẵn có cho các yêu cầu từ bất kỳ miền nào ngay khi người dùng có token hợp lệ.

## Tiêu chuẩn cơ bản
Khi tạo token, bạn có một vài tùy chọn. Chúng ta sẽ đi sâu hơn vào chủ đề này khi chúng ta bảo mật một API trong một bài viết tiếp theo, nhưng tiêu chuẩn để sử dụng sẽ là JSON Web Tokens.

Biểu đồ thư viện và trình gỡ lỗi tiện dụng này hiển thị hỗ trợ cho các thẻ JSON Web. Bạn có thể thấy rằng nó có một số lượng lớn hỗ trợ trên nhiều ngôn ngữ. Điều này có nghĩa là bạn có thể thực sự chuyển đổi cơ chế xác thực của mình nếu bạn chọn làm như vậy trong tương lai!

## kết luận
Đây chỉ là cái nhìn tổng quát cách thức và lý do xác thực dựa trên token. Như trong thế giới bảo mật hiện tại, có nhiều, rất nhiều, cực kỳ nhiều, nhiều lắm (quá nhiều?) Nhiều hơn cho mỗi chủ đề và nó thay đổi theo từng trường hợp sử dụng. Thậm chí, chúng ta còn nghiên cứu một số chủ đề về khả năng mở rộng cũng xứng đáng với cuộc trò chuyện của riêng họ.

Đây là mô hình tổng quan nâng cao ,  vì vậy xin vui lòng chỉ ra bất cứ điều gì đã bị bỏ lỡ hoặc bất kỳ câu hỏi nào bạn có về vấn đề này.

Trong bài viết tiếp theo của chúng ta, chúng ta sẽ xem xét cụ thể JSON Web Tokens. Để biết các ví dụ về mã đầy đủ về cách xác thực API Node bằng cách sử dụng JSON Web Tokens, hãy xem cuốn sách MEAN Machine của chúng tôi.

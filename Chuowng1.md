#Chương 1: Sự phát triển của dữ liệu

#Mục lục:
**Table of Content**

- [1. Nhu cầu lưu trữ hiện nay](#1)
	- [1.1 Sự tăng trưởng của dữ liệu: Exabytes, Hellabytes, and Beyond](#11)
	- [1.2 Yêu cầu của việc lưu trữ Unstructured data](#12)
- [2. No One-Size-Fits-All Storage System](#2)
- [3. So sánh với những loại Storage khác](#3)
- [4. Kiến trúc lưu trữ kiểu mới: SDS](#4)
- [5. Các thành phần của SDS](#5)
- [6. Tại sao lại lựa chọn Swift?](#6)
- [7. Kết luận](#7)

--------------------------------------------------------

##Mở đầu:
- Năm 2011, Openstack Object Storage gọi là Swift được giới thiệu như là 1 sản phẩm rất tốt để giải quyết vấn đề về sự gia tăng dữ liệu 1 cách nhanh chóng. 
- SDS với hệ thống như là Openstack Swift được phát triển, Object storage và SDS được xem như là bước logic tiếp theo trong sự phát triển của lưu trữ.
- Trước khi đi vào Swift, phần này sẽ nói về sự bùng nổ dữ liệu unstructured và sự so sánh Object storage với Block storage và filestorage.

----------------------------------------------------------------------------

<a name="1"></a>
##1. Nhu cầu của việc lưu trữ hiện nay:
Ngày nay nhu cầu sử dụng và truy cập dữ liệu của các công ty và tổ chức tăng cao, người sử dụng cũng tạo ra nhiều kiểu dữ liệu gọi là unstructured data 
như online video, gaming, Software-as-a-Service...Khiến cho hệ thống lưu trữ cần được mở rộng để phát triển và truy cập dễ dàng. Unstructures data là những dữ liệu không có 1 cấu trúc nhất
 định như là image, videos, documents...được lưu trữ dưới dạng file trái với kiểu dữ liệu được truy cập trong database (thường là dữ liệu có cấu trúc). 
 Những dữ liệu này được tạo ra bởi rất nhiều các thiết bị được kết nối với internet
 
 
<a name="11"></a>
###1.1 Sự tăng trưởng của dữ liệu: Exabytes, Hellabytes, and Beyond:
Nghiên cứu của International Data Corporation (IDC) năm 2013 về quy mô của dữ liệu. Theo nghiên cứu này, dữ liệu có thể sẽ tăng từ 2,596 exabytes (EB) năm  2012 tới 7,235 EB năm 2017. 
Tới năm 2020, số lượng dữ liệu có thể sẽ tăng tới 35840 BE, chia cho dân số thế giới sẽ vào khoảng 4terabytes/1 người. Độ lớn của 1 exabytes tương đương với 1000 petabytes(PB), 1 triệu
terabytes và 1 tỉ gigabytes(GB). 1 ví dụ đơn giản nếu so sánh 1 cuấn sách tương đương với 1 megabytes, thư viện lớn nhất chứa 23 triệu cuốn sách tương đương khoảng 23 TB dữ liệu vậy 
sẽ cần khoảng 43000 thư viện có kích thước như vậy để chứa 1 exabytes. 1 ví dụ khác nếu 1 bức ảnh chất lượng cao là 2MB thì 1 exabyte sẽ chứa khoảng 537 tỉ bức ảnh. Tải trọng lưu trữ càng ngày càng lớn hơn, 
ngoài 1 exabytes (10^18 bytes) còn có zettabyte (1021 bytes), yottabyte(10^24 bytes) theo International System of Units (IS).


<a name="12"></a>
###1.2 Yêu cầu của việc lưu trữ Unstructured data:
Lưu trữ unstructured data cần đảm bảo được 4 yếu tố sau: Durability, Availability, Manageability, Low cost.
- <b>Durability</b>:
	Durability là độ bền của dữ liệu, độ bền là để đánh giá về sự an toàn của dữ liệu không bị mất đi hoặc bị hư hỏng bất kể do lỗi của cá nhân bởi vì có rất nhiều unstructured data cần được lưu trữ trong thời gian dài hoặc có thể là mãi mãi bởi những yêu cầu về quy định và pháp lý

- <b>Availability</b>:
	Availability là tính sẵn sàng của dữ liệu, tính sẵn sàng là thời gian hoạt động và sự đáp ứng được của hệ thống khi gặp phải lỗi hay là tải trọng nặng của dữ liệu bởi vì người dùng muốn dữ liệu của họ có thể truy cập trên bất kì thiết bị ở mọi nơi

- <b>Manageability</b>:
	Manageability là khả năng quản lý, khả năng quản lý là sự yêu cầu về những mức độ quản lý, tùy vào những hệ thống nhỏ hoặc lớn sẽ có những phương án quản lý cho phù hợp về personal, time, những nguy cơ, sự linh hoạt. Đối với những hệ thống lớn, khả năng quản là rất quan trọng, hệ thống cần được quản lý 1 cách đơn giản và phù hợp.

- <b>Low cost</b>:
	Với nguồn tài chính lớn, các vấn đề về hệ thống lưu trữ sẽ được giải quyết, tuy nhiên do có nhiều hạn chế nên việc lưu trữ cần có giá thành rẻ. Những công ty hiện nay cần biết những chi phí để đầu tư cho hệ thống lưu trữ 


<a name="2"></a>
##2. No One-Size-Fits-All Storage System:
Giải pháp one-size-fits-all là rất hay tuy nhiên mỗi hệ thống lưu trữ cần có giải pháp riêng phù hợp với yêu cầu và hoàn cảnh cụ thể theo định lý CAP. 
Định lý CAP nói rằng hệ thống máy tính phân tán không đồng thời cũng cấp được: Consistency, Availability, Partition tolerance

- <b>Consistency</b>
Tính nhất quán. Tất cả khách hàng có thể thấy cùng 1 phiên bản của dữ liệu trong cùng khoảng thời gian
Ví dụ trong 1 hệ thống có nhiều nodes, dữ liệu trên các nodes sẽ được đồng bộ với nhau

- <b>Availability</b>
Tính sẵn sàng. Khi đọc hoặc viết trên hệ thống cần đảm bảo được phản hồi
Ví dụ trong hệ thống máy tính phân phối có nhiều nodes, khi có 1 node bị hỏng thì hệ thống vẫn hoạt động bình thường, khách hàng vẫn có thể truy cập vào dữ liệu

- <b>Partition tolerance</b>:
Khả năng phân vung. Hệ thống làm việc khi mạng hoạt động không ổn định
Ví dụ giữa các nodes mất kết nối với nhau thì hệ thống vẫn phải hoạt động bình thường

 Khi triển khai thực tế 1 hệ thống máy tính phân tán, ta sẽ phải chọn 2 đặc tính trên bởi vì partition tolerance không thực sự quan trọng  trong hệ thống máy tính phân tán, nó sẽ kết hợp với hoặc Consistency hoặc Availability tùy vào mục đích sử dụng của thống. Việc xây dựng hệ thống lưu trữ phục vụ cho 1 khối lượng công việc cụ thể sẽ tốt hơn là xây dựng để hỗ cho tất cả khối lượng công việc.
 
Swift đảm bảo được tính sẵn có và khả năng chịu phân vùng. Nghĩa là Swift sẽ xử lý khối lượng công việc cần thiết để lưu trữ 1 lượng lớn các dữ liệu phi cấu trúc. Điều này cho phép Swift rất bền vững và có tính sẽ sàng rất cao







 
 





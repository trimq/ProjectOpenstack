#Chương 1: Sự phát triển của dữ liệu

#Mục lục:
**Table of Content**

- [1. Nhu cầu lưu trữ hiện nay](#1)
	- [1.1 Sự tăng trưởng của dữ liệu: Exabytes, Hellabytes, and Beyond](#11)
	- [1.2 Yêu cầu của việc lưu trữ Unstructured data](#12)
- [2. No One-Size-Fits-All Storage System](#2)
- [3. So sánh Object storage với những loại lưu trữ khác](#3)
- [4. Kiến trúc lưu trữ kiểu mới: SDS](#4)
- [5. Các thành phần của SDS](#5)
	- [5.1 Lợi ích của SDS](#51)
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

Khi triển khai thực tế 1 hệ thống máy tính phân tán, ta sẽ phải chọn 2 trên 3 đặc tính trên bởi vì partition tolerance không thực sự quan trọng  trong hệ thống máy tính phân tán, nó sẽ kết hợp với hoặc Consistency hoặc Availability tùy vào mục đích sử dụng của thống. Việc xây dựng hệ thống lưu trữ phục vụ cho 1 khối lượng công việc cụ thể sẽ tốt hơn là xây dựng để hỗ cho tất cả khối lượng công việc.
 
Swift đảm bảo được tính sẵn có và khả năng chịu phân vùng. Nghĩa là Swift sẽ xử lý khối lượng công việc cần thiết để lưu trữ 1 lượng lớn các dữ liệu phi cấu trúc. Điều này cho phép Swift rất bền vững và có tính sẽ sàng rất cao


<a name="3"></a>
##3. So sánh Object storage với những loại lưu trữ khác:

Dó có nhiều kiểu dữ liệu khác nhau vì thế mỗi kiểu dữ liệu cần có 1 cách thức lưu trữ phù hợp, có 3 kiểu lưu trữ như: Block, File, Object.

-<b>Block storage</b>
Lưu trữ structured data (thể hiện bằng các khối dữ liệu bằng nhau, 2^12 bits 1 block), các databases thường sử dụng phổ biến kiểu lưu trữ này, filesystem thường được sử dụng trong lưu trữ khối

-<b>File storage</b>
File storage cung cấp khả năng lưu trữ unstructured data tuy nhiên filestorage cần 1 sự nhất quán cao do đó có nhiều hạn chế khi sử dụng trong các hệ thống có số lượng lớn dữ liệu nên thường áp dụng trong mô hình nhỏ (như máy PC).

-<b>Object storage</b>
Object không sử dụng cách truy cập như block, Object truy cập thông qua các API, truy cập qua URLs thông qua giao thức HTTP. Lợi ích của Object storage là sự phân phối các yêu cầu, đảm bảo cho những hệ thống quy mô lớn mà giá thành vẫn rẻ, đây là 1 giải pháp rất tốt để giải quyết việc dữ liệu tăng lên nhanh chóng hiện nay.

Hệ thống object storage sẽ được cung cấp 1 single namespace, giúp cho hệ thống không cần chia nhỏ dữ liệu và gửi các phần đó tới các nơi lưu trữ khác nhau. Việc này giúp hệ thống có khả năng quản lý rất tốt và tận dụng được tối đa nguồn lực lưu trữ.

<a name="4"></a>
##4. Kiến trúc lưu trữ kiểu mới: SDS (Software-Define Storage)

Lịch sử của việc lưu trữ dữ liệu trong các mainframe, sau đó các thiết bị lưu trữ này được tách ra khỏi mainframe sử dụng 1 bộ điều khiến, tuy nhiên là với việc những dữ liệu ngày càng nhiều đã phá vỡ cấu trúc của 1 bộ điều khiển có thể quản lý vì vậy cần có 1 kiến trúc lưu trữ kiểu mới để có thể làm việc được với nhiều dữ liệu như hiện nay

Các hệ thống lưu trữ trước đây thường chạy trên các phần cứng được tùy chỉnh và sử dụng dụng phần mềm đóng, điều này làm cho việc bảo trì và di chuyển trở nên khó khăn và cần phải kiểm soát rất chặt chẽ để ngăn ngừa hỏng hóc

Quy mô của unstructured cần 1 sự thay đổi lớn trong cấu trúc lưu trữ dữ liệu. SDS bắt đầu được sử dụng trong việc lưu trữ dữ liệu, SDS được tạo ra để đáp ứng được 4 nhu cầu của lưu trữ unstructured data như durability, availability, low cost, manageability

SDS đặt ra sự đảm bảo cho hệ thống trong phần mềm, SDS thay vì kiểm ngăn chặn lỗi thì nó sẽ chấp nhận lỗi xảy ra trong quá trình hoạt động và làm việc thông qua hoặc xung quanh những lỗi đó

Dữ liệu Unstructured data đã vượt hơn structure data về tổng lượng lưu trữ và lợi nhuận mang lại nên giải pháp lưu trữ sử dụng SDS, giải pháp này giúp cho hệ thống có thể mở rộng, tăng khả năng chịu lỗi, có thể kết hợp nhiều phần cứng khác nhau trong 1 hệ thống

<a name="5"></a>
##5. Các thành phần của SDS:
Thành phần của SDS, bao gồm 4 thành phần Storage routing, Storage resilience, Physical hardware, Out-of-band controller

-<b>Storage routing</b>:
<ul>
<li>Đóng vai trò như 1 gateway để truy cập vào hệ thống lưu trữ, pools of routers và những dịch vị truy cập vào router có thể phân phối ra nhiều data center. Có thể thể mở rộng quy mô để tăng lượng dữ liệu có thể truy cập</li>
<li>Thành phần này sẽ định tuyến những request hoạt động tránh khỏi những lỗi về phần cứng hay networking bị lỗi, nó sẽ sử dụng những cơ chế đơn giản để sao chép dữ liệu khỏi vùng bị lỗi</li>
<li>Trong quá trình hoạt động của 1 hệ thống SDS , nó sẽ bật những giao thức để hỗ trợ và phản hồi lại qua các API</li>
</ul>

-<b>Storage resilience (khả năng phục hồi)</b>:
<ul>
<li>Hệ thống SDS phục hồi lại khi có sự cố là trách nhiệm của phần mềm không phải phần cứng, có nhiều cách để bảo vệ dữ liệu đảm bảo rằng dữ liệu sẽ không hỏng hoặc mất</li>
<li>Có thể có những quy trình kiểm tra riêng biệt để xem các dữ liệu được bảo vệ trên các node lưu trữ như nào, nếu như phát hiện ra lỗi hệ thống sẽ có biện pháp để xử lý</li>
</ul>

-<b>Physical hardware</b>:
Các phần cứng vật lý lưu trữ các bits trên đĩa, các nodes lưu trữ dữ liệu không chịu trách nhiệm đảm bảo độ bền của dữ liệu mà nó là nhiệm vụ của Storage resilience

-<b>Out-of-band controller</b>:
Có vai trò điều phối truy cập vào các nodes lưu trữ, tách biệt ra khỏi hệ thống lưu trữ. Có thể tự động điều chỉnh để tăng hiệu suất làm việc của các nodes như tăng khả năng phục hồi dữ liệu khi có nodes bị fail, sắp xếp tài nguyên trên các node lưu trữ, kết nối mạng, định tuyến và dịch vụ cho toàn bộ cụm 

<a name="51"></a>
###5.1 Lợi ích của SDS:

- SDS mang lại 1 sự hoạt động và quản lý hiệu quả bởi vì mỗi thành phần trong đó đều là của 1 hệ thống máy tính phân tán. Sự sắp xếp này giúp cho việc mở rộng, nâng cấp được thực hiện mà không bị downtime hay phải di chuyển dữ liệu đi

- SDS tách biệt phần cứng ra khỏi phần mềm giúp cho việc sử dụng nhiều loại ổ cứng trong cùng 1 hệ thống, cho phép hệ thống có thể tích hợp những công nghệ mới

-  Giải pháp SDS thường là mã nguồn mở, giúp cho người dùng có nhiều công cụ và tránh việc phải bó buộc vào 1 nhà sản xuất. Giúp cho chúng ta có thể xây dựng các ứng dụng có thể tương thích với nhiều phần cứng hơn.














 
 





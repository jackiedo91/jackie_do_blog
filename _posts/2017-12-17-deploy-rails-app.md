---
layout: post
title: "[Server] - Deploy a Rails app"
date:   2017-12-17 02:00:00
categories: server
published: true
---

<blockquote><p><strong>
Hôm nay chúng ta sẽ deploy một web app được viết bằng Rails lên một VPC (ở đây mình dùng EC2 của AWS).
</strong></p></blockquote>

<h2>Tạo một VPC trên AWS</h2>
Sau khi login vào Amazon Web Server
<ol>
<strong><li> Chọn mục EC2 trong category Compute </li></strong>
<strong><li> Dưới mục "Create Instance" chọn Launch Instance </li></strong>
<strong><li> Ở đây ta sẽ có tất cả 7 bước để tạo một Instance (có thể hiểu đơn giản là một VPC) </li></strong>
  <br/>
  - Step 1: Choose an Amazon Machine Image (AMI) 
  <br/>
  Chọn Ubuntu Server 16.04 -> Select
  <br/>
  <br/>
  - Step 2: Choose an Instance Type 
  <br/>
  Chọn t2 micro (Free tier eligible). Note: đối với loại free thì chỉ có thể tạo ở Region "Ohio" hoặc "Virginia"
  -> Next: Configure Instance Detail
  <br/>
  <br/>
  - Step 3: Configure Instance Details
  <br/>
  Giữ nguyên các mặc định -> Next: Add Storage
  <br/>
  <br/>
  - Step 4: Add Storage
  <br/>
  Nhập dung lượng bạn muốn sử dụng (thường là 30Gb) -> Next: Add tags
  <br/>
  <br/>
  - Step 5: Add tags
  <br/>
  Giữ nguyên các mặc định -> Next: Configure Security Group
  <br/>
  <br/>
  - Step 6: Configure Security Group
  <br/>
  Mặc định thì instance này chỉ có thể truy cập SSH vào từ bên ngoài qua port 22.
  Bạn cần Add thêm Rule để cho phép các loại truy cập khác (HTTP hoặc HTTPS) để có thể truy cập web server từ bên ngoài qua port 80.
  Trong bài này ta sẽ dùng HTTP với các config mặc định.
  -> Review and Launch
  <br/>
  <br/>
  - Step 7: Review Instance Launch
  <br/>
  Xem lại các thông số và cảnh báo, với các set up trên thì instance này có thể được truy cập từ bất cứ đâu.
  -> Launch (Done việc set up)
  <br/>
  <br/>
  Note: Khi setup một instance, Amazon sẽ cung cấp cho bạn một pem key. bạn cần download pem key này để truy cập SSH lên server.
  <br/>
  <br/>
<strong><li> Các thông tin cần lưu ý của một instance </li></strong>
  <br/>
  Public DNS (IPv4) : [Your Amazon domain]
  <br/>
  IPv4 Public IP : [Your IP]
  <br/>
  Key pair name : [Your key pem name]
  <br/>
</ol>


<h2>Tạo một Rails app</h2>

<h2>Config trên server VPS để chuẩn bị cho việc deploy</h2>


<h2>Một vài câu lệnh hay dùng trong Postgresql</h2>

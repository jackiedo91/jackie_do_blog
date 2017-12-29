---
layout: post
title: "[Server] - Deploy a Rails app to AWS"
date:   2017-12-17 02:00:00
categories: server
published: false
---

<blockquote><p><strong>
Hôm nay chúng ta sẽ deploy một web app được viết bằng Rails dùng database Postgresql lên một VPC (ở đây mình dùng EC2 của AWS), theo thứ tự bên dưới:
<ul>
  <li>Tạo một VPC trên AWS</li>
  <li>Tạo một Rails local app</li>
  <li>Config trên server VPS để chuẩn bị cho việc deploy</li>
  <li>Một vài câu lệnh hay dùng trong Postgresql</li>
</ul>
</strong></p></blockquote>

<h2>Tạo một VPC trên AWS</h2>
Sau khi login vào Amazon Web Server
<ol>
<strong><li> Chọn mục EC2 trong category Compute </li></strong>
<strong><li> Dưới mục "Create Instance" chọn Launch Instance </li></strong>
<strong><li> Ở đây ta sẽ có tất cả 7 bước để tạo một Instance (có thể hiểu đơn giản là một VPC) </li></strong>
  <br/>
  - <u>Step 1:</u> Choose an Amazon Machine Image (AMI) 
  <br/>
  Chọn Ubuntu Server 16.04 -> Select
  <img src="{{ site.baseurl }}/assets/server/step_1.png" alt="Choose an Amazon Machine Image"/>
  <br/>
  <br/>
  - <u>Step 2:</u> Choose an Instance Type 
  <br/>
  Chọn t2 micro (Free tier eligible). Note: đối với loại free thì chỉ có thể tạo ở Region "Ohio" hoặc "Virginia"
  -> Next: Configure Instance Detail
  <img src="{{ site.baseurl }}/assets/server/step_2.png" alt="Choose an Instance Type"/>
  <br/>
  <br/>
  - <u>Step 3:</u> Configure Instance Details
  <br/>
  Giữ nguyên các mặc định -> Next: Add Storage
  <img src="{{ site.baseurl }}/assets/server/step_3.png" alt="Configure Instance Details"/>
  <br/>
  <br/>
  - <u>Step 4:</u> Add Storage
  <br/>
  Nhập dung lượng bạn muốn sử dụng (thường là 30Gb) -> Next: Add tags
  <img src="{{ site.baseurl }}/assets/server/step_4.png" alt="Add Storage"/>
  <br/>
  <br/>
  - <u>Step 5:</u> Add tags
  <br/>
  Giữ nguyên các mặc định -> Next: Configure Security Group
  <img src="{{ site.baseurl }}/assets/server/step_5.png" alt="Add tags"/>
  <br/>
  <br/>
  - <u>Step 6:</u> Configure Security Group
  <br/>
  Mặc định thì instance này chỉ có thể truy cập SSH vào từ bên ngoài qua port 22.
  Bạn cần Add thêm Rule để cho phép các loại truy cập khác (HTTP hoặc HTTPS) để có thể truy cập web server từ bên ngoài qua port 80.
  Trong bài này ta sẽ dùng HTTP với các config mặc định.
  -> Review and Launch
  <img src="{{ site.baseurl }}/assets/server/step_6.png" alt="Configure Security Group"/>
  <br/>
  <br/>
  - <u>Step 7:</u> Review Instance Launch
  <br/>
  Xem lại các thông số và cảnh báo, với các set up trên thì instance này có thể được truy cập từ bất cứ đâu.
  -> Launch (Done việc set up)
  <img src="{{ site.baseurl }}/assets/server/step_7.png" alt="Review Instance Launch"/>
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
  Key pair name : [Your pem key name]
  <br/>
  <img src="{{ site.baseurl }}/assets/server/step_8.png" alt="Server Info"/>
</ol>

<h2>Tạo một Rails app</h2>

<h2>Config trên server VPS để chuẩn bị cho việc deploy</h2>


<h2>Một vài câu lệnh hay dùng trong Postgresql</h2>

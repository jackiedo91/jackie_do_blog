---
layout: post
title: "[Ruby] Design patterns in ruby - Template method"
date:   2019-05-10 02:00:00
categories: ruby
published: true
---

<blockquote><p><strong>Template Method pattern cho chúng ta khả năng thống nhất đồng thời cho phép tùy biến các chức năng có chung một luồng xử lý bằng cách sử dụng một abstract base class, định nghĩa các template method với các methods khung bên trong nó nhờ đó các class kế thừa sẽ có luồng xử lý thống nhất và có thể tùy biến dựa vào các yêu cầu cụ thể.</strong></p></blockquote>

<h2>Bài toán</h2>
<p>Chúng ta có một class được dùng để xuất ra một report với định dạnh HTML. Sau một khoảng thời gian, ngoại trừ chức năng xuất repot HTML ban đầu thì bây giờ còn phát sinh thêm chức năng mới "xuất report bằng text" dùng chung data input được sử dụng cho HTML report. </p>

<p>Có nhiều cách để làm được chức năng mới này: tạo một class với cấu trúc giống giống với HTML report class, đặt câu lệnh if/else vào class cũ để tùy chọn xuât HTML hay text ...</p>

<p>Theo chủ để của bài blog ngày hôm nay, chúng ta sẽ refactor code lại áp dụng Template Method để tiện cho việc quản lý và cập nhật sau này, đồng thời cũng tạo ra sự thống nhất giữa các report (dùng chung input, dùng chung các methods ...)</p>

<blockquote>
<h4>Report Class ban đầu (HTML report)</h4>

{% highlight ruby %}
  class Report
    def initialize
      @title = 'Monthly Report'
      @text = [ 'Things are going', 'really, really well.' ]
    end

    def output_report
      puts('<html>')
      puts('  <head>')
      puts("    <title>#{@title}</title>")
      puts('  </head>')
      puts('  <body>')
        @text.each do |line|
          puts("  <p>#{line}</p>")
        end
      puts('  </body>')
      puts('</html>')
    end

  end
{% endhighlight %}
</blockquote>


<br/>
<h3><i>[Bad Way] Hướng xử lý sử dụng điều kiện để tùy chọn xuất HTML hoặc Text</i></h3>

<blockquote>
<h4>Report Class - thêm if/else</h4>

{% highlight ruby %}
  class Report
    def initialize
      @title = 'Monthly Report'
      @text = [ 'Things are going', 'really, really well.' ]
    end

    def output_report(format)
      if format == :text
        puts("*** #{@title} ***")
      elsif format == :html
        puts('<html>')
        puts('  <head>')
        puts("    <title>#{@title}</title>")
        puts('  </head>')
        puts('  <body>')
      else
        raise "Unknow format: #{format}"
      end

      @text.each do |line|
        if format == :text
          puts(line)
        else
          puts("  <p>#{line}</p>")
        end
      end

      if format == :html
        puts('  </body>')
        puts('</html>')
      end
    end

  end
{% endhighlight %}
</blockquote>

<br/>
<h3><i>[Good Way] Hướng xử lý áp dụng Template Method</i></h3>

<blockquote>
<p>Cấu trúc của class cơ sở ban đầu sẽ có một luồng chức năng cơ bản (business basic flow) và không nên bị thay đổi bởi các yêu cầu phát sinh sau này: xuất text, xuất PDF... </p>
<h4><b> "Abstract base class" ban đầu:
<br/>
- Đây là một abstract class, không đặc tả bất cứ method nào, các method xử lý cụ thể được đặc tả bằng cách overwrite lại ở các class con.
<br/>
- Input Data được định nghĩa và xử lý tại đây, cách class con sẽ dùng chung input data.</b></h4>
{% highlight ruby %}
  class Report
    def initialize
      @title = 'Monthly Report'
      @text = [ 'Things are going', 'really, really well.' ]
    end

    # This is the template method of this base class
    def output_report
      output_start
      output_head
      output_body_start
      output_body
      output_body_end
      output_end
    end

    def output_body
      @text.each do |line|
        output_line(line)
      end
    end

    def output_start
      raise 'Called abstract method: output_start'
    end

    def output_head
      raise 'Called abstract method: output_head'
    end

    def output_body_start
      raise 'Called abstract method: output_body_start'
    end

    def output_line(line)
      raise 'Called abstract method: output_line'
    end

    def output_body_end
      raise 'Called abstract method: output_body_end'
    end

    def output_end
      raise 'Called abstract method: output_end'
    end

  end
{% endhighlight %}

<br/>
<h4>HTML report version</h4>
{% highlight ruby %}
  class HTMLReport < Report

    def output_start
      puts('<HTML>')
    end

    def output_head
      puts('<head>')
      puts("  <title>#{@title}</title>")
      puts('</head>')
    end

    def output_body_start
      puts('<body>')
    end

    def output_line(line)
      puts("<p>#{line}</p>")
    end

    def output_body_end
      puts('</body>')
    end

    def output_end
      puts('</html>')
    end

  end
{% endhighlight %}

<br/>
<h4>Plain text report version</h4>
{% highlight ruby %}
  class PlainTextReport < Report

    def output_start
      # we need to set unused methods are blank to avoid raising exceptions.
    end

    def output_head
      puts("**** #{@title} ****")
    end

    def output_body_start
    end

    def output_line(line)
      puts(line)
    end

    def output_body_end
    end

    def output_end
    end

  end
{% endhighlight %}
<br/>
<p style="text-align: center;"> ==================== Sử dụng các class mới để xuất reports ==================== </p>
{% highlight ruby %}
  report = HTMLReport.new
  report.output_report

  report = PlainTextReport.new
  report.output_report
{% endhighlight %}
</blockquote>

<br/>
<h2>Cấu trúc - Template Method</h2>
<img src="{{ site.baseurl }}/assets/ruby/template_method_diagram.png" alt="Template Method Diagram"/>

<br/>
<h2>Ưu nhược điểm của Template Method</h2>
<h3><i><u>Ưu điểm</u></i></h3>
<ul>
<li>Đa dạng hóa và tùy biến một chức năng.</li>
<li>Có thể cập nhật thêm mới các chức năng mới mà không ảnh hưởng cấu trúc ban đầu.</li>
<li>Dễ dàng cho việc viết unit tests.</li>
</ul>

<h3><i><u>Nhược điểm</u></i></h3>
<ul>
<li>Cần phải thiết kế  tốt abstract base class ban đầu để tránh phải sửa chữa sau này.</li>
<li>Dùng chung input data format giữa các class con.</li>
<li>Sau khi khởi tạo một instance của một trong các class con thì method bị cố định (không linh hoạt bằng <a href="{{ site.baseurl }}/ruby/2017/12/03/template-method-stategy-in-ruby.html">Strategy Design Pattern</a>).
</li>
</ul>


<br/>
<p style="text-align: center;"> ==================== Phần Mở Rộng ==================== </p>

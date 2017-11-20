---
layout: post
title: Design patterns in ruby - Template method
date:   2017-11-04 02:00:00
categories: ruby
published: false
---

<blockquote><p><strong>Công dụng của Template Method pattern là xây dựng một abstract base class với một bộ 'khung' có sẵn từ đó các class kế thừa sẽ thống nhất và sử dụng chung khung này .</strong></p></blockquote>

<h2>Bài toán</h2>
<p>Chúng ta có một class được dùng để xuất ra một report với định dạnh HTML. Sau một khoảng thời gian, ngoại trừ chức năng xuất repot HTML ban đầu thì bây giờ còn phát sinh thêm chức năng mới "xuất report bằng text" với đúng data input được sử dụng cho HTML report. </p>

<p>Có nhiều cách để làm được chức năng mới này: tạo một class với cấu trúc xêm xêm với HTML report class, đặt câu lệnh if/else vào class cũ để tùy chọn xuât HTML hay text ...</p>

<p>Theo tiêu đề của bài chúng ta sẽ refactor code lại áp dụng Template Method để tiện cho việc quản lý và cập nhật sau này, đồng thời cũng tạo ra sự thống nhất giữa các report (dùng chung input, dùng chung các methods ...)</p>

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



<h4>Report Class - sử dụng if/else để tùy chọn xuất HTML or Text (cách chuối nhất)</h4>

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

### "Abstract class" ban đầu

{% highlight ruby %}
  class Report
    def initialize
      @title = 'Monthly Report'
      @text = [ 'Things are going', 'really, really well.' ]
    end

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

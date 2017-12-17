---
layout: post
title: "[Ruby] Design patterns in ruby - Observer"
date:   2017-12-07 02:00:00
categories: ruby
published: false
---

<blockquote><p><strong>Bài trước đã nói về cách tùy biến/đa dạng hóa một luồng xử lý bằng Template Method, mỗi khi chúng ta muốn thêm một cách xử lý mới, chúng ta tạo thêm một class con(sub-class) dựa trên abtract base class ban đầu. Tuy nhiên, cách này có hạn chế cố hữu vì dựa đa phần trên việc kế thừa (inheritance): không quan trọng bạn thiết kế abtract base class (super-class) kỹ lưỡng tới đâu, mối quan hệ giữa sub-class và super-class sẽ rối dần theo thời gian, khi hệ thống phình to lên. Ngoài ra thì Template Method sẽ fix cứng cách xử lý tại "runtime", một khi đã khởi tạo một instance và truyền input data vào thì instance đó chỉ làm việc theo đúng design của một sub-class nào đó, nếu bạn muốn làm gì đó khác đi với cùng input data đó, bạn phải tạo nguyên một instance mới của một sub-class khác.
<br/>
<br/>
Bài này chúng ta vẫn giải quyết bài toán tùy biến/đa dạng hóa một luồng xử lý nhưng với một pattern khác - Strategy và nó có thể tránh đi một vài khuyết điểm của Template Method. Với Strategy thay vì chúng ta gọi trực tiếp một method(template method) ở abtract base class để xuất ra report thì ta có một class trung gian để làm việc này (context class). Tại class trung gian, ta chỉ việc truyền vào input data và lựa chọn "chiến thuật-strategy" để xử lý data này, chiến thuật có thể thay đổi ngay sau khi tạo ra một instance của class này, hay nói cách khác nó có thể thay đổi chiến thuật ngay tại lúc runtime.
</strong></p></blockquote>

<h2>Bài toán</h2>
<p>
Vẫn bài toán xuất report với nhiều format khác nhau (HTML, plain text ...), nhưng ta sẽ thử áp dụng Strategy Pattern.
</p>

<blockquote>
<h4>Report Class - Context Class</h4>

{% highlight ruby %}
  class Report
    attr_reader :title, :text
    attr_accessor :formatter

    def initialize(formatter)
      @title = 'Monthly Report'
      @text = [ 'Things are going', 'really, really well.' ]
      @formatter = formatter
    end

    def output_report
      @formatter.output_report(@title, @text)
    end

  end
{% endhighlight %}

<h4>Strategy base class (đặt tên class gợi nhớ về chức năng)</h4>

{% highlight ruby %}
  class Formatter
    def output_report(title, text)
      raise 'Called abtract method: output_report'
    end
  end
{% endhighlight %}

<h4>HTML Strategy class (chiến thuật số một: xuất HTML)</h4>

{% highlight ruby %}
  class HTMLFormatter < Formatter
    def output_report(title, text)
      puts('<html>')
      puts('  <head>')
      puts("    <title>#{title}</title>")
      puts('  </head>')
      puts('  <body>')
        text.each do |line|
          puts("  <p>#{line}</p>")
        end
      puts('  </body>')
      puts('</html>')
    end
  end
{% endhighlight %}

<h4>Plain text Strategy class (chiến thuật số hai: xuất Text)</h4>

{% highlight ruby %}
  class PlainTextFormatter < Formatter
    def output_report(title, text)
      puts("***** #{title} *****")
      text.each do |line|
        puts(line)
      end
    end
  end
{% endhighlight %}

<p style="text-align: center;"> ==================== Sử dụng Strategy ==================== </p>
{% highlight ruby %}
  # Dùng chiến thuật xuất HTML tại thời điểm khởi tạo
  report = Report.new(HTMLFormatter.new)
  report.output_report

  # Đối qua chiến thuật xuất Plain Text
  report.formatter = PlainTextFormatter.new
  report.output_report

{% endhighlight %}

</blockquote>
<br/>
<h3><i>[Update] Tối ưu một chút để pass tất cả data từ Context Class qua Strategy Classes</i></h3>
<blockquote>
<h4>Report Class - Context Class</h4>

{% highlight ruby %}
  class Report
    attr_reader :title, :text
    attr_accessor :formatter

    def initialize(formatter)
      @title = 'Monthly Report'
      @text = [ 'Things are going', 'really, really well.' ]
      @formatter = formatter
    end

    #Pass all data of Context Class
    def output_report
      @formatter.output_report(self)
    end

  end
{% endhighlight %}

<h4>Strategy base class</h4>

{% highlight ruby %}
  class Formatter
    def output_report(context)
      raise 'Called abtract method: output_report'
    end
  end
{% endhighlight %}

<h4>HTML Strategy class</h4>

{% highlight ruby %}
  class HTMLFormatter < Formatter
    def output_report(context)
      puts('<html>')
      puts('  <head>')
      puts("    <title>#{context.title}</title>")
      puts('  </head>')
      puts('  <body>')
        context.text.each do |line|
          puts("  <p>#{line}</p>")
        end
      puts('  </body>')
      puts('</html>')
    end
  end
{% endhighlight %}

<h4>Plain text Strategy class (chiến thuật số hai: xuất Text)</h4>

{% highlight ruby %}
  class PlainTextFormatter < Formatter
    def output_report(context)
      puts("***** #{context.title} *****")
      context.text.each do |line|
        puts(line)
      end
    end
  end
{% endhighlight %}

</blockquote>
<br/>
<h2>Cấu trúc - Strategy</h2>
<img src="{{ site.baseurl }}/assets/ruby/strategy_diagram.png.png" alt="Strategy Diagram"/>

<br/>
<h2>Ưu nhược điểm của Strategy</h2>
<h3><i><u>Ưu điểm</u></i></h3>
<ul>
<li>Tách biệt data(context class) và cách xử lý (strategy classes), nhờ đó ta có thể dễ dàng thay đổi cách xử lý tại thời điểm runtime.</li>
</ul>

<h3><i><u>Nhược điểm</u></i></h3>
<ul>
<li>Phải tìm cách pass data qua các strategy classes.</li>
</ul>

<br/>


<p style="text-align: center;"> ==================== Phần Mở Rộng ==================== </p>

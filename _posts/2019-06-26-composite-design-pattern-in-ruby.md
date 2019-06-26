---
layout: post
title: "[Ruby] Design patterns in ruby - Composite"
date:   2019-06-26 02:00:00
categories: ruby
published: true
---

> **Chia để trị là chiến lược quen thuộc khi lập trình để giải quyết một bài toán. Trong các design patterns thì có một desgin pattern tận dụng triệt để chiến lược chia để trị đó là `Composite` <br/> <br/> `Composite` như cái tên của nó là sự kết hợp nhiều mảnh riêng biệt, mỗi mảnh sẽ giải quyết một nhiệm vụ cụ thể và khi ta tổng hợp các mảnh lại ta sẽ có đáp án cho bài toán lớn cần giải.**

## Cấu trúc - Composite

<img src="{{ site.baseurl }}/assets/ruby/composite_diagram.png" alt="Composite Diagram"/>


## Bài toán
---
Tìm các sinh viên dựa trên một số đặc điểm, thông tin địa chỉ, điểm số của các sinh viên.

{% capture text-capture %}
```ruby
  student = {
    name: {
      first: 'Khoa',
      last: 'Pham',
      middle: 'Hoang'
    },
    gender: 'male',
    birth_day: '1999-04-12',
    school_info: {
      school: 'Dai Hoc HCM',
      department: 'Cong Nghe Thong Tin',
      major: 'He Thong Thong Tin'
    },
    address_info: {
      country: 'Viet Nam',
      city: 'Ho Chi Minh',
      district: 'Quan 5',
      street: '48 Bui Thi Xuan'
    },
    subjects: [
      {
        name: 'Toan',
        score: 10
      },
      {
        name: 'Ly',
        score: 9
      },
      {
        name: 'Hoa',
        score: 8
      }
    ]
  }
```
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="student-toggle" button-text=" Cấu trúc của student " toggle-text=text-capture %}


{% capture text-capture %}
```ruby
  students = [
    {
      id: 1,
      name: {
        first: 'Khoa',
        last: 'Pham',
        middle: 'Hoang'
      },
      gender: 'male',
      birth_day: '1999-04-12',
      school_info: {
        school: 'Dai Hoc Ho Chi Minh',
        department: 'Cong Nghe Thong Tin',
        major: 'He Thong Thong Tin'
      },
      address_info: {
        country: 'Viet Nam',
        city: 'Ho Chi Minh',
        district: 'Quan 5',
        street: '48 Bui Thi Xuan'
      },
      subjects: [
        {
          name: 'Toan',
          score: 10
        },
        {
          name: 'Ly',
          score: 9
        },
        {
          name: 'Hoa',
          score: 8
        }
      ]
    },
    {
      id: 2,
      name: {
        first: 'Tuan',
        last: 'Bui',
        middle: 'Anh'
      },
      gender: 'male',
      birth_day: '1991-09-09',
      school_info: {
        school: 'Dai Hoc Hue',
        department: 'Moi Truong',
        major: 'Xu Ly Rac Thai'
      },
      address_info: {
        country: 'Viet Nam',
        city: 'Hue',
        district: 'An Dong',
        street: '50 An Dong'
      },
      subjects: [
        {
          name: 'Toan',
          score: 5
        },
        {
          name: 'Ly',
          score: 5
        },
        {
          name: 'Hoa',
          score: 5
        }
      ]
    },
    {
      id: 3,
      name: {
        first: 'Ha',
        last: 'Ho',
        middle: 'Ngoc'
      },
      gender: 'female',
      birth_day: '1989-01-01',
      school_info: {
        school: 'Dai Hoc Ho Chi Minh',
        department: 'Kinh Te',
        major: 'Quan Tri Kinh Doanh'
      },
      address_info: {
        country: 'Viet Nam',
        city: 'Ho Chi Minh',
        district: 'Q2',
        street: '12 Nguyen Duy Trinh'
      },
      subjects: [
        {
          name: 'Toan',
          score: 8
        },
        {
          name: 'Ly',
          score: 3
        },
        {
          name: 'Hoa',
          score: 2
        }
      ]
    },
    {
      id: 4,
      name: {
        first: 'Hoa',
        last: 'Nguyen',
        middle: 'Thi'
      },
      gender: 'female',
      birth_day: '1995-06-03',
      school_info: {
        school: 'Dai Hoc Ho Chi Minh',
        department: 'Cong Nghe Thong Tin',
        major: 'Lap Trinh'
      },
      address_info: {
        country: 'Viet Nam',
        city: 'Ho Chi Minh',
        district: 'Q3',
        street: '101 Le Van Sy'
      },
      subjects: [
        {
          name: 'Toan',
          score: 9
        },
        {
          name: 'Ly',
          score: 9
        },
        {
          name: 'Hoa',
          score: 9
        }
      ]
    },
    {
      id: 5,
      name: {
        first: 'Hung',
        last: 'Dam',
        middle: 'Vinh'
      },
      gender: 'female',
      birth_day: '1985-07-07',
      school_info: {
        school: 'Dai Hoc Ha Noi',
        department: 'Luat',
        major: 'Luat Kinh Te'
      },
      address_info: {
        country: 'Viet Nam',
        city: 'Ha Noi',
        district: 'Cau Giay',
        street: '120 Tran Duy Hung'
      },
      subjects: [
        {
          name: 'Toan',
          score: 5
        },
        {
          name: 'Ly',
          score: 5
        },
        {
          name: 'Hoa',
          score: 5
        }
      ]
    }
  ]

```
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="student-list-toggle" button-text=" Data mẫu cho 5 students " toggle-text=text-capture %}

Dựa vào cấu trúc của student, ta có thể tìm kiếm student thông qua 6 tiêu chí:
  * `name`
  * `gender`
  * `birth day`
  * `school info`
  * `address info`
  * `subject`

Với mỗi tiêu chí ta lại có một format hơi khác biệt để tìm kiếm, cụ thể  hơn hãy theo các ví dụ tiếp theo bên dưới.

---
## Phân tích
---
Dựa vào cấu trúc tổng quan của `Composite` ta có thể triển khai bài toán như sau:
* Leaf: một leaf sẽ tìm dựa trên duy nhất 1 tiêu chí. Ta sẽ có 6 leaves: `Name`, `Gender`, `BirthDay`, `SchoolInfo`, `AddressInfo` and `Subject`
* Composite: là nơi sẽ nhận data theo chuẩn input format của các leaves, initialize cho các leaves và thực thi việc tạo. Ta có 1 composite: `SearchCompositor`
* Component: Nơi nhận data từ đâu đó và format data theo input format của các leaves (hiện tại giả sử data theo chuẩn format). Ta có 1 component gốc: `Seach`
* Ngoài ra ta còn có 2 template classes, 1 để xây dựng cấu trúc cho các leaves `BaseLeaf`, 1 để xây dựng cấu trúc cho các composites `BaseCompositor`

---


### Case 1: Tìm kiếm dựa trên `name`
> Giả sử đổi với `name`, ta cần tìm kiếm tên với nhiều chế độ khác nhau (options) như: tìm kiếm chính xác(correct), tìm kiếm khớp phần đầu(prefix), tìm kiếm khớp một phần(partial).

* **Input:**

```ruby
# Tìm kiếm chính xác
conditions = [
  {
    type: 'name',
    input: {
      first: 'Khoa',
      last: 'Pham',
      middle: 'Hoang'

    },
    options: {
      match_mode: 'correct'
    }
  }
]

# Tìm kiếm khớp phần đầu
conditions = [
  {
    type: 'name',
    input: {
      first: 'H'
    },
    options: {
      match_mode: 'prefix'
    }
  }
]


# Tìm kiếm khớp một phần
conditions = [
  {
    type: 'name',
    input: {
      first: 'o'
    },
    options: {
      match_mode: 'partial'
    }
  }
]

```

* **Cấu trúc code sẽ triển khai**
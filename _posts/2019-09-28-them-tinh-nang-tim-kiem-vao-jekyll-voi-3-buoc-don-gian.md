---
layout: post
title: Thêm Tính Năng Tìm Kiếm Vào Jekyll Với 3 Bước Đơn Giản
categories: Jekyll
toc: true
---
Việc tìm kiếm một nội dung cụ thể nào đó có thể khó khăn nếu một trang web không có tính năng tìm kiếm. Trang web của mình không có tính năng tìm kiếm trước đây vì mình có rất ít bài viết và trang. Cuối cùng, khi số lượng bài viết đó đã tăng lên, mình đã chịu thua khi tìm kiếm ngay cả một đoạn nhỏ trong một số bài viết mà mình đã viết.

### Tại sao một blog Jekyll cần tính năng tìm kiếm?

Nếu trang Jekyll của bạn chỉ có 4 đến 5 trang thì bạn sẽ không cần tính năng tìm kiếm, bởi lẽ tất cả các trang đó có thể được liệt kê liên kết trong một thanh menu hoặc hiển thị trên chân trang. Tuy nhiên đối với một blog Jekyll với rất nhiều bài viết thì phải cần đến một thanh tìm kiếm. Có rất nhiều tùy chọn có sẵn để thêm thanh tìm kiếm vào trong Jekyll như:

* Lunr
* Google Custom Search Engine
* Simple Jekyll Search

> Đánh giá nhanh: Lunr rất nhanh chóng nhưng lại rất nặng. Google Custom thì lại nhẹ nhưng mà lại hoạt động chậm chạp. Chỉ có Simple Jekyll Search vừa nhẹ lại vừa nhanh chóng.

Qua đánh giá trên thì các bạn biết trong bài này mình sẽ nói về cái nào rồi hén :D. Simple Jekyll Search được phát triển bởi **christian-fei**. Cho đến nay nó là công cụ tìm kiếm nhanh chóng và dễ dàng nhất có sẵn dành cho Jekyll. Bản quyền cũng thuộc về **Alex Pearce**, người đã đăng ý tưởng của về nó.

Đây là một bản demo đơn giản của Simple Jekyll Search giúp tìm từ khóa trong 5 bài đăng gần đây: [Ấn vào đây để xem demo](https://blog.webjeda.com/demo/instant-jekyll-search/).

### Làm thế nào để thêm Jekyll Instant Search?

Vì Jekyll không có thực thi được với phía máy chủ nên chúng ta phải dựa vào việc lưu trữ tất cả nội dung cần thiết trong một tệp và tìm kiếm từ khóa của chúng ta từ tệp đó.

Chúng ta sẽ tạo một tệp JSON, trong đó chúng ta sẽ lưu trữ tiêu đề trang, liên kết trang, danh mục, thẻ, mô tả, v.v., JSON (JavaScript Object Notation: Ký hiệu đối tượng JavaScript) là cách một cách mà đối với con người thì nó rất là dễ đọc, để lưu trữ dữ liệu theo cặp giá trị khóa (Có thể có hình thức khác tốt hơn). Ví dụ:

```
[
    {
     "Name": "BROOK"
    }
]
```
#### Bước 1: Tạo một tệp JSON

Đầu tiên bạn hãy vào thư mục gốc và tạo một file có tên là `search.json` có nội dung sau đây:

```
---
---
[{% raw %}
  {% for post in site.posts %}
    {

      "title"    : "{{ post.title | escape }}",
      "url"      : "{{ site.baseurl }}{{ post.url }}",
      "category" : "{{ post.category }}",
      "tags"     : "{{ post.tags | join: ', ' }}",
      "date"     : "{{ post.date }}"

    } {% unless forloop.last %},{% endunless %}
  {% endfor %}
{% endraw %}]
```

Những gì nó làm là nó chuyển đổi dữ liệu Jekyll của bạn từ tất cả các bài đăng và đặt nó làm cặp giá trị chính mà sau đó có thể dễ dàng được đọc bởi một tập lệnh tìm kiếm.

Nếu bạn đang dùng thử trên máy cục bộ thì bạn có thể xác minh `search.json` trong thư mục `_site` để xem liệu tất cả các giá trị có được tạo hay không.

Kết quả sẽ trông giống như thế này:

```
[
    {
      "title"    : "Materialized",
      "url"      : "/materialized/",
      "category" : "cards",
      "image"    : "materialized.png",
      "dev"      : "lpsandaruwan"
    }    ,

    {
      "title"    : "Time-machine",
      "url"      : "/time-machine/",
      "category" : "onepage",
      "image"    : "time-machine.png",
      "dev"      : "pages-themes"
    }
]
```

Đây là một mẫu đơn giản từ một trang web của mình, nơi mình đã thêm tính năng tìm kiếm.

Các mã trên có thể được thay đổi (cẩn thận đừng làm hỏng nó) theo nhu cầu của bạn. Xóa bất cứ trường nào không cần thiết cho trang web Jekyll của bạn. Thêm nếu bạn muốn bao gồm một số yếu tố khác hoặc nội dung của bài viết, mô tả, danh mục như dưới đây.

```
---
---
[{% raw %}
  {% for post in site.posts %}
    {

      "title"    : "{{ post.title | strip_html | escape }}",
      "url"      : "{{ site.baseurl }}{{ post.url }}",
      "category" : "{{post.categories | join: ', '}}",
      "tags"     : "{{ post.tags | join: ', ' }}",
      "date"     : "{{ post.date }}",
      "discription" : "{{post.description | strip_html | strip_newlines | escape }}"

    } {% unless forloop.last %},{% endunless %}
  {% endfor %}
{% endraw %}]
```

Bộ lọc `escape` rất quan trọng bởi vì nếu bạn có một dấu ngoặc kép `"` bên trong giá trị của `post.description` thì toàn bộ JSON sẽ bị hỏng và không sử dụng được. Mình cũng khuyên bạn không nên sử dụng toàn bộ nội dung của bài viết để tạo JSON vì nó có thể dẫn đến việc tạo một tệp có kích thước rất lớn.

Bạn cũng có thể thêm `post.content` vào đây nhưng mình khuyên bạn không nên. Thêm nhiều nội dung sẽ dẫn đến một tệp JSON nặng và kết quả tìm kiếm sẽ không chính xác.

> Đảm bảo rằng giá trị cuối cùng không có dấu phẩy `,` ở cuối.

Từ những gì mình đã trải nghiệm, giá trị bạn đặt lên đầu sẽ được ưu tiên. Trong ví dụ trên, tiêu đề sẽ được ưu tiên cao nhất và đoạn trích được thấp nhất. Thay đổi điều này theo nhu cầu của bạn.

#### Bước 2: Tạo tệp script

Vào trang [này](https://raw.githubusercontent.com/christian-fei/Simple-Jekyll-Search/master/dest/simple-jekyll-search.min.js) sao chép toàn bộ và lưu với tên `search-script.js` (hoặc bất kỳ tên nào) tại trang của bạn.

> Sẽ tốt hơn nếu bạn tạo thư mục `js` và bỏ nó vào.

#### Bước 3: Tạo trang tìm kiếm

Tạo file `search.html` (hoặc bất kỳ tên nào và dán code sau:

```
<!-- Giao diện HTML -->
<div id="search-container">
<input type="text" id="search-input" placeholder="search...">
<ul id="results-container"></ul>
</div>

<!-- Đường dẫn tới search-script.js -->
<script src="/js/search-script.js" type="text/javascript"></script>

<!-- Tùy chỉnh -->
<script>
SimpleJekyllSearch({
  searchInput: document.getElementById('search-input'),
  resultsContainer: document.getElementById('results-container'),
  json: '/search.json'
})
</script>
```

Thành thật mà nói thì bước này không cần thiết lắm. Bạn có thể thêm code đó vào layout hoặc trang chủ của bạn. Tại sao mình không muốn thêm tìm kiếm trên tất cả các trang là bởi vì, tính năng tìm kiếm không được sử dụng bởi tất cả người dùng mọi lúc nên không có lý do để gọi tập lệnh tìm kiếm (khá lớn) trên tất cả các trang. Điều này có thể ảnh hưởng đến thời gian tải trang của bạn.

Chú ý chỗ đường dẫn cho tệp script và tệp JSON. Nếu bất kỳ một trong số chúng sai thì bạn sẽ không nhận được bất kỳ kết quả nào.

Mình khuyên bạn không nên sử dụng cái này trên layout (được sử dụng trên mỗi trang) vì nó có thể dẫn đến tải trang chậm hơn. Hãy sử dụng một trang tìm kiếm riêng để thay thế.

Vậy là xong! Hãy thử nhập một cái gì đó và xem kết quả nếu nó tìm nạp bất kỳ kết quả nào.

Nếu tìm kiếm không hoạt động thì hãy sử dụng tùy chọn console » trên trình duyệt Chrome để xem lỗi là gì. Nhiều lần JSON được tạo sẽ không hợp lệ vì có một số ký tự đặc biệt. Ngoài ra, thứ tự mà tập lệnh tìm kiếm cũng có thể gây ra vấn đề.

### Tùy chỉnh

Ở mặc định thì kết quả nó sẽ trông như này:

```
<li><a href="{url}">{title}</a></li>
```

Bạn có thể thay đổi nó bằng cách thêm dòng này vào đoạn Javascript,

```
searchResultTemplate: '<div><a href="{url}"><h1>{title}</h1></a><span>{date}</span></div>',
```

Ở đây {url}, {title}, {date} là dữ liệu tương ứng được tìm thấy trong tệp JSON.

Bạn cũng có thể hiển thị thông báo khi không tìm thấy kết quả bằng cách thêm dòng này vào tập lệnh cấu hình:

```
noResultsText: 'Không tìm thấy kết quả!',
```

> Hãy nhớ rằng, sau cùng là dấu phẩy `,` thì không ở cuối cùng. Ở cuối cùng thì sau cùng không có dấu phẩy.

Có nhiều tùy chọn khác có thể tùy chỉnh. Truy cập [vào đây](https://github.com/christian-fei/Simple-Jekyll-Search) để biết thêm.

### Tổng kết

Có thể có một vài sai sót trong khi triển khai Simple Jekyll Search và nó có thể không nhận được kết quả. Tin mình đi, mình đã từng bị. Đừng bỏ cuộc, hãy làm theo từng bước mà không bỏ bước nào. Sử dụng công cụ Google Chrome Inspect » Console để xem  sai sót ở chỗ nào.

Tìm kiếm tức thì chắc chắn sẽ giúp người xem của bạn tìm thấy những gì họ muốn kiếm dễ dàng hơn và nhanh chóng hơn. Yếu tố này sẽ gián tiếp giúp giảm tỷ lệ thoát trang của bạn.

Chúc bạn thành công!

Được dịch và chỉnh sửa dựa trên bài viết: https://blog.webjeda.com/instant-jekyll-search.
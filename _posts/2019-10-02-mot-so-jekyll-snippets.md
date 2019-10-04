---
layout: post
title: Một Số Jekyll Snippets
categories: jekyll
toc: true
---
Hôm nay mình sẽ chia sẻ một số Jekyll Snippets hữu ích có thể được chèn trực tiếp vào trang web Jekyll mà không cần chỉnh sửa gì hết.

Ở bài này, ngoài `html` và `liquid` thì mình cũng đã thêm một style. Bạn có thể phải sửa lại một số giá trị như chiều rộng, màu sắc, v.v., theo nhu cầu của bạn.

Bạn luôn có thể chuyển style sang `stylesheet` hoặc đưa lên `head`. Nhưng với style nội tuyến theo cách này sẽ tăng tốc thời gian tải trang.

### Jekyll Pagination

```
{% raw %}
<!-- 
     Trước khi thêm Jekyll snippet này, hãy đảm bảo:	
      1. Bạn đã thêm dòng "plugin: jekyll-pagination" tệp _config.yml.
      2. Sử dụng vòng lặp for có "paginator.post" thay vì "site.posts".
-->

{% if paginator.total_pages > 1 %}
<div class="wj-pagination"> 
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }}">&laquo; Prev</a>
  {% else %}
    <span>&laquo; Prev</span>
  {% endif %}

  {% for page in (1..paginator.total_pages) %}
    {% if page == paginator.page %}
      <span class="active">{{ page }}</span>
    {% elsif page == 1 %}
      <a href="/">{{ page }}</a>
    {% else %}
      <a href="{{ site.paginate_path | prepend: site.baseurl | replace: '//', '/' | replace: ':num', page }}">{{ page }}</a>
    {% endif %}
  {% endfor %}

  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }}">Next &raquo;</a>
  {% else %}
    <span>Next &raquo;</span>
  {% endif %}
</div>
{% endif %}

<style>
.wj-pagination {
    text-align: center;
}
.wj-pagination a, .wj-pagination span {
    padding: 7px 18px;
    border: 1px solid #eee;
    margin-left: -2px;
    margin-right: -2px;
    background-color: #ffffff;
    display: inline-block;
}
.wj-pagination a:hover {    
    background-color: #f1f1f1;
    color: #333;
}
</style>
{% endraw %}
```

### Form liên hệ

```
{% raw %}
<!-- 
     Sau khi thêm mẫu form liên hệ này, hãy đảm bảo:
      1. Bạn đã thêm dòng "email: youremail@email.com" vào trong tệp _config.yml.
      2. Bạn đã xác minh form trên formspree.io.
-->

<form class="wj-contact" action="https://formspree.io/{{site.email}}" method="POST">
    <input type="text" name="email" placeholder="Email Address">
    <textarea type="text" name="content" rows="10" placeholder="Message"></textarea>
    <input type="hidden" name="_next" value="<REDIRECTION LINK> ">
    <input type="hidden" name="_subject" value="New Contact Form Submission">
    <input type="text" name="_gotcha" style="display:none">
    <input type="submit" value="Submit">
</form>

<style>
form.wj-contact input[type="text"], form.wj-contact textarea[type="text"] {
    width: 100%;
    vertical-align: middle;
    margin-top: 0.25em;
    margin-bottom: 0.5em;
    padding: 0.75em;
    font-family: monospace, sans-serif;
    font-weight: lighter;
    border-style: solid;
    border-color: #444;
    outline-color: #2e83e6;
    border-width: 1px;
    border-radius: 3px;
    transition: box-shadow .2s ease;
}

form.wj-contact input[type="submit"] {
    outline: none;
    color: white;
    background-color: #2e83e6;
    border-radius: 3px;
    padding: 0.5em;
    margin: 0.25em 0 0 0;
    border: 1px solid transparent;
    height: auto;
}
</style>
{% endraw %}
```

### Form đăng ký nhận bài mới

```
{% raw %}
<!-- 
     Trước khi thêm đoạn jekyll snippet này, hãy đảm bảo:
     1. Bạn đã đăng ký tài khoản ở Mailchimp.
     2. Bạn đã tạo một danh sách ở Mailchimp.
     3. Bạn đã thêm danh sách đó vào _config.yml,
     Ví dụ: "mailchimp-list: //redgadgets.us10.list-manage.com/subscribe/post?u=210acce5db69d3d4a04b0e25d&amp;id=08c6708f40"
-->  

<form action="{{site.mailchimp-list}}" method="post" name="mc-embedded-subscribe-form" class="wj-contact-form validate" target="_blank" novalidate>
    <div class="mc-field-group">
        <input type="email" placeholder="Email" name="EMAIL" class="required email" id="mce-EMAIL" autocomplete="on">
        <input type="submit" value="Subscribe" name="subscribe" class="heart">
    </div>
</form>

<style>
    .wj-contact-form input {
        vertical-align: middle;
        margin-top: 0.25em;
        margin-bottom: 0.5em;
        padding: 0.75em;
        font-family: monospace, sans-serif;
        border:1px solid #444;
        outline-color: #2e83e6;
        border-radius: 3px;
        transition: box-shadow .2s ease;
    }
    
    .wj-contact-form input[type="submit"] {
        background-color: #2e83e6;
        border: 1px solid #2e83e6;;
        color: #eee;
    }
</style>
{% endraw %}
```

### Mục lục

Mình đã thiết kế nó để trông giống như Mục lục của Wikipedia. Bạn có thể tạo các đoạn css trong thẻ style để cá nhân hóa cho trang web của bạn.

```
{% raw %}
<!-- 
     Trước khi thêm đoạn jekyll snippet này, hãy đảm bảo:
     1. Bạn đã thêm dòng "markdown: kramdown" vào _config.yml.
-->


* Không xóa dòng này (nó sẽ không hiển thị)
{:toc}


<style>
#markdown-toc::before {
    content: "Contents";
    font-weight: bold;
}

#markdown-toc ul {
    list-style: decimal;
}

#markdown-toc {
    border: 1px solid #aaa;
    padding: 1.5em;
    list-style: decimal;
    display: inline-block;
}
</style>
{% endraw %}
```

### Disqus lazy load

Việc tải thêm bình luận chỉ có ý nghĩa khi người dùng cuộn xuống phía dưới. Mã này sẽ tự động phát hiện, cuộn và tải thêm bình luận của Disqus khi bạn chạm đến cuối màn hình.

```
{% raw %}
<!-- 
     Trước khi thêm đoạn jekyll snippet này, hãy đảm bảo:
     1. Gửi lời cảm ơn đến css-tricks.com
     2. Thêm dòng "disqus: DISQUS-SHORTNAME" vào trong tệp _config.yml.
-->

<div class="disqus"></div>
<div class="disqus-loading">Loading comments&hellip;</div>
<style>
 .disqus-placeholder.is-hidden { display: none; }
</style>
<script>
/*
	disqusLoader.js v1.0
	A JavaScript plugin for lazy-loading Disqus comments widget.
	-
	By Osvaldas Valutis, www.osvaldas.info
	Available for use under the MIT License
*/

(function(b,f,l){var r=function(a){a=a.getBoundingClientRect();return{top:a.top+f.body.scrollTop,left:a.left+f.body.scrollLeft}},t=function(a,c){var d=f.createElement("script");d.src=a;d.async=!0;d.setAttribute("data-timestamp",+new Date);d.addEventListener("load",function(){"function"===typeof c&&c()});(f.head||f.body).appendChild(d)};l=function(a,c){var d,e;return function(){var g=this,f=arguments,b=+new Date;d&&b<d+a?(clearTimeout(e),e=setTimeout(function(){d=b;c.apply(g,f)},a)):(d=b,c.apply(g,
f))}};var m=!1,n=!1,p=!1,k=!1,h="unloaded",e=!1,q=function(){if(!e||!f.body.contains(e)||"loaded"==e.disqusLoaderStatus)return!0;var a=b.pageYOffset,c=r(e).top;if(c-a>b.innerHeight*n||0<a-c-e.offsetHeight-b.innerHeight*n)return!0;(a=f.getElementById("disqus_thread"))&&a.removeAttribute("id");e.setAttribute("id","disqus_thread");e.disqusLoaderStatus="loaded";"loaded"==h?DISQUS.reset({reload:!0,config:p}):(b.disqus_config=p,"unloaded"==h&&(h="loading",t(k,function(){h="loaded"})))};b.addEventListener("scroll",
l(m,q));b.addEventListener("resize",l(m,q));b.disqusLoader=function(a,c){var d={laziness:1,throttle:250,scriptUrl:!1,disqusConfig:!1},b=c,g,h={};for(g in d)Object.prototype.hasOwnProperty.call(d,g)&&(h[g]=d[g]);for(g in b)Object.prototype.hasOwnProperty.call(b,g)&&(h[g]=b[g]);c=h;n=c.laziness+1;m=c.throttle;p=c.disqusConfig;k=!1===k?c.scriptUrl:k;e="string"===typeof a?f.querySelector(a):"number"===typeof a.length?a[0]:a;e.disqusLoaderStatus="unloaded";q()}})(window,document,0); 
</script>
<script>
	disqusLoader( '.disqus',
	{
		scriptUrl:		'//{{site.disqus}}.disqus.com/embed.js',
		disqusConfig:	function()
		{
			this.page.identifier 	= '{{page.url}}';
			this.page.url			= '{{page.url | prepend: site.baseurl}}';
			this.page.title			= '{{page.title}}';
			this.callbacks.onReady	= [function()
			{
				var el = document.querySelector( '.disqus-loading' );
				if( el.classList )
					el.classList.add( 'is-hidden' );
				else
					el.className += ' ' + 'is-hidden';
			}];
		}
	});
</script>
{% endraw %}
```

### Nút chia sẻ lên mạng xã hội

```
{% raw %}
<link href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet">
<style>
.share-box a {
  display: inline-block;
  -webkit-box-shadow: 0 0 1px #777;
  box-shadow: 0 0 1px #777;
  padding: 5px 12px;
  margin-right: 5px;
  margin-bottom: 5px;
  text-decoration: none; }
  .share-box a:hover {
    text-decoration: none;
    -webkit-transition: background-color 200ms linear;
    -ms-transition: background-color 200ms linear;
    transition: background-color 200ms linear; }

.f {
  color: #3b5998; }
  .f:hover {
    color: #fff;
    background-color: #3b5998; }

.t {
  color: #4099FF; }
  .t:hover {
    color: #fff;
    background-color: #4099FF; }

.g {
  color: #d34836; }
  .g:hover {
    color: #fff;
    background-color: #d34836; }

.r {
  color: #ff5700; }
  .r:hover {
    color: #fff;
    background-color: #ff5700; }

.l {
  color: #0077b5; }
  .l:hover {
    color: #fff;
    background-color: #0077b5; }

.e {
  color: #444444; }
  .e:hover {
    color: #fff;
    background-color: #444444; }
</style>

<div class="share-box">
<h5>Share this:</h5>

<a class="f" href="https://www.facebook.com/sharer/sharer.php?u={{ site.url }}{{site.baseurl}}{{ page.url }}" onclick="window.open(this.href, 'mywin',
'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;" ><i class="fa fa-facebook-official fa"></i><span> facebook</span></a>

<a class="t" href="https://twitter.com/intent/tweet?text={{ page.title }}&url={{ site.url }}{{site.baseurl}}{{ page.url }}" onclick="window.open(this.href, 'mywin',
'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;"><i class="fa fa-twitter fa"></i><span> twitter</span></a>

<a class="g" href="https://plus.google.com/share?url={{ site.url }}{{site.baseurl}}{{ page.url }}" onclick="window.open(this.href, 'mywin',
'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;" ><i class="fa fa-google-plus fa"></i><span> google</span></a>

<a class="r" href="http://www.reddit.com/submit?url={{ site.url }}{{site.baseurl}}{{ page.url }}" onclick="window.open(this.href, 'mywin',
'left=20,top=20,width=900,height=500,toolbar=1,resizable=0'); return false;" ><i class="fa fa-reddit fa"></i><span> reddit</span></a>

<a class="l" href="https://www.linkedin.com/shareArticle?mini=true&url={{ site.url }}{{site.baseurl}}{{ page.url }}&title={{ page.title }}&summary={{ page.desc }}&source=webjeda" onclick="window.open(this.href, 'mywin',
'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;" ><i class="fa fa-linkedin fa"></i><span> linkedin</span></a>

<a class="e" href="mailto:?subject={{ page.title }}&amp;body=Check out this site {{ site.url }}{{site.baseurl}}{{ page.url }}"><i class="fa fa-envelope fa"></i><span> email</span></a>                          
</div>
{% endraw %}
```

### Thời gian đọc bài

```
{% raw %}
<!-- 
    Trước khi sử dụng đoạn jekyll snippet này, hãy đảm bảo:
    1. Đừng quan tâm nếu IDE hiển thị lỗi
    2. Có thể thêm từ "Thời gian đọc". Hiện tại, nó sử dụng một icon từ https://github.com/danklammer/bytesize-icons
-->

<span class="read-time" title="Estimated read time">
  <svg id="i-clock" viewBox="0 0 32 32" width="20" height="20" fill="none" stroke="currentcolor" stroke-linecap="round" 
  stroke-linejoin="round" stroke-width="2"><circle cx="16" cy="16" r="14" /><path d="M16 8 L16 16 20 20" /></svg>       

  {% assign words = content | number_of_words %}
  {% if words < 360 %}
    1 min read.
  {% else %}
    {{ words | divided_by:180 }} mins read.
  {% endif %}
</span>
<style>
    svg#i-clock {vertical-align: middle;}
</style>
{% endraw %}
```

> *"Mày không thoát được đâu con trai! Tu bi không tình yêu..."*

Được dịch và chỉnh sửa dựa trên bài viết: https://blog.webjeda.com/jekyll-snippets.
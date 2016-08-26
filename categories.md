---
layout: default
title: "分类：Categories"
---
<ul class="list-unstyled">
{% for cat in site.categories %} 
	{% if cat[0] != 'blog' %} 
   <a name="{{ cat[0] }}"></a>
   <h3 style="color:#ffffff; background:#888888">{{ cat[0] }}&nbsp; <span style="color:ffffff; font-size:13px">[{{ cat[1].size }}]</span></h3>
     {% for post in cat[1] %} 
    <li><h4><span>{{ post.date | date_to_string }}</span> &nbsp; &bull; &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></h4></li>
	{% endfor %} 
   {% endif %} 
{% endfor %} 
</ul>

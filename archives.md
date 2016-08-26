---
layout: default
title: "归档：Archives"
---

<ul class="list-unstyled">
     {% for post in site.posts limit:100 %} 
	 {% unless post.next %} 
    <h2>{{ post.date | date: '%Y' }}</h2> 
	{% else %} {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %} {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %} 
	{% if year != nyear %}

    <h2>{{ post.date | date: '%Y' }}</h2> {% endif %} 
	{% endunless %} 
    <li style="line-height:30px;"><h4 style="padding-top:5px;"><span style="color:#555555">{{ post.date | date_to_string }}</span>&nbsp; &bull; &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></h4></li> 
	{% endfor %} 
</ul> 

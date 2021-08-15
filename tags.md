---
layout: post
title: Tags
permalink: /tags/
standalone: true
---

<div id="archives">
 <ul>
{% for tag in site.tags %}
  <div class="archive-group">
	{% capture tag_name %}{{ tag | first }}{% endcapture %}
   <div id="#{{ tag_name | slugize }}"></div>
   <p></p>
   
   <h3 class="archives-head">{{ tag_name }}</h3>
   <a name="{{ tag_name | slugize }}"></a>

   	{% for post in site.tags[tag_name] %}
   <li> <article class="archive-item">
     <h5><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h5>
    </article></li>
    {% endfor %}
    
   </div>
{% endfor %}
 </ul>
</div>
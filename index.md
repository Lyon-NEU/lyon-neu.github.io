---
layout: page
title: Hello
tagline: zhaosen
---
{% include JB/setup %}




<!-- ## Pages List -->
<!-- <ul>
{% for member in site.data.members %}
  <li>
    <a href="https://github.com/{{ member.github }}">
      {{ member.name }}
    </a>
  </li>
{% endfor %}
</ul> -->

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a> 
    <!-- <p>{{post.excerpt}}</p> -->
    </li>
  {% endfor %}
</ul>
<!-- {% for category in site.categories %}
<h3>{{ category | first }}</h3> 
<ul class="arc-list">
{% for post in category.last %} 
<li>{{ post.date | date:"%Y/%m/%d"}}<a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</ul> 
{% endfor %} -->



  



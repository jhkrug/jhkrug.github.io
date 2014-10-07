---
layout: page
title: About & Archive
---

John Krug. I work at [Lancaster University](http://www.lancaster.ac.uk)
in the [Library](http://lancaster.ac.uk/library) in the Digital Innovation
team where I attempt to do things with library systems and analytics
and maybe some linked data.

<a href="/atom.xml">RSS</a>

###Previously
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.date | date: "%d/%m/%Y" }} - {{ post.title }}</a>
    </li>
  {% endfor %}
</ul>


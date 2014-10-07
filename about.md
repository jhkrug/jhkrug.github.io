---
layout: page
title: About & Archive
---

John Krug. I work at [Lancaster University](http://www.lancaster.ac.uk)
in the [library](http://lancaster.ac.uk/library) where I attempt to do
things with library systems and analytics and maybe some linked data.

###Previously
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.date | date: "%d/%m/%Y" }} - {{ post.title }}</a>
    </li>
  {% endfor %}
</ul>


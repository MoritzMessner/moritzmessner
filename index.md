---
layout: default
title: Blog - Moritz Messner
---

Willkommen auf meinem Blog! Hier schreibe ich über Flutter-Entwicklung, Mobile Apps und Technologie.

---

## Neueste Artikel

{% for post in site.posts limit:5 %}
<div class="entry" markdown="1">

### [{{ post.title }}]({{ post.url | relative_url }})

<span class="entry-date">{{ post.date | date: "%d.%m.%Y" }}</span>

{{ post.description }}

</div>
{% endfor %}

{% if site.posts.size == 0 %}
<p class="text-muted">Noch keine Blogposts vorhanden.</p>
{% endif %}

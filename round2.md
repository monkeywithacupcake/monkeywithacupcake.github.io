---
layout: default
title: Round 2
permalink: /round2/
pagination: 
  enabled: true
  collection: round2
  trail: 
    before: 2 # The number of links before the current page
    after: 2  # The number of links after the current page
---

 <ul class="post-list">
    {% for post in paginator.posts %}
      <li>
        <h2>
          <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
         </h2>
        {% assign date_format = site.minima.date_format | default: "%Y-%m-%d" %}
        <span class="post-meta">{{ post.date | date: date_format }}
            {% capture words %}
              {{ post.content | number_of_words | minus: 250 }}
            {% endcapture %}
            {% unless words contains "-" %}
              {{ words | plus: 250 | divided_by: 180 | append: " min read" }}
            {% endunless %}
          </span>
              {{ post.excerpt | strip_html | truncatewords:30 }}        
      </li>
    {% endfor %}
  </ul>

{% if paginator.total_pages > 1 %}
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path | prepend: site.baseurl }}"> Newer  </a>
  {% endif %}
  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path | prepend: site.baseurl }}"> Older </a>
  {% endif %}
{% endif %}





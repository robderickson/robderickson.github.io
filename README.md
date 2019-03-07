{% for post in site.posts %}
  <h2>[{{ post.title }}]({{ post.url }})</h2>  
  {{ post.excerpt }}
{% endfor %}
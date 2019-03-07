# I'm Probably Doing This Wrong
{% for post in site.posts %}
[{{ post.title }}](###{{ post.url }})  
{{ post.published_at | date: "%Y-%m-%d" }}
{{ post.excerpt }}  
{% endfor %}
# I'm Probably Doing This Wrong
{% for post in site.posts %}
[{{ post.title }}]({{ post.url }})  
----------------------------------
{{ post.date | date: "%Y-%m-%d" }}
{{ post.excerpt }}  
{% endfor %}
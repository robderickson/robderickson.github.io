# I'm Probably Doing This Wrong
{% for post in site.posts %}
[{{ post.title }}](## {{ post.url }})  
{{ post.date }}
{{ post.excerpt }}  
{% endfor %}
# I'm Probably Doing This Wrong
Where I show you how I probably did things the hard way.

{% for post in site.posts %}
[{{ post.title }}](**{{ post.url }}**)  
{{ post.date | date: "%Y-%m-%d" }}  
{{ post.excerpt }}  
{% endfor %}
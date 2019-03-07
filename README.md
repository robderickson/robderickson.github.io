<h1>I'm Probably Doing This Wrong</h1>
{% for post in site.posts %}
<h3>[{{ post.title }}]({{ post.url }})</h3>  
<p>{{ post.excerpt }}</p>  
{% endfor %}
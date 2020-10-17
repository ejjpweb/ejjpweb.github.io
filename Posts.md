---
layout: page
title: Posts
---

<html>
	<br>
	<ul class="posts">

	  {% for post in site.posts %}
	    <span>{{ post.date | date_to_string }}</span>  <br><a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a> <br><br>
	  {% endfor %}
	
	</ul>
	
</html>

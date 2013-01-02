---
layout: page
title: Programação Funcional
tagline: F#, Haskell, Clojure e.. engenharia, matemática e física..
---
{% include JB/setup %}


{% assign p = site.posts.first %}
<div id="post">
  <h2 class="post_title"><a href="{{ p.url }}">{{ p.title}}</a></h2>
  <hr/>
  <p class="post-date"><span class="date">{{ p.date | date: "%d/%m/%Y" }}</span></p>
  <p class="post-date"></p>
  {{ p.content }}
  <h3><a href="{{ p.url }}#comments">Comentários</a></h3>
</div>
<hr/>

<h3>Posts recentes</h3>
<ul class="posts">
    {% for post in site.posts limit:5 offset:0 %}
        <li>
        	<p>
        		<span class="date">{{ post.date | date: "%d/%m/%Y" }}</span> &raquo;
        		<a href="{{ post.url }}">{{ post.title }}</a>
        	</p>
        	</li>
    {% endfor %}
</ul>

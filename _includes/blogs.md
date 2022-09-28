<section class="thirteen columns" markdown="1">
# Blogs
<p>
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
</p>
</section>
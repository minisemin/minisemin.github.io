---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---


<!-- Category buttons -->
<div id="category-buttons">
  <button data-category="all">All</button>
  {% assign all_categories = site.posts | map: "categories" | join: "," | split: "," | uniq | sort %}
  {% for cat in all_categories %}
    <button data-category="{{ cat | slugify }}">{{ cat }}</button>
  {% endfor %}
</div>

<!-- Post list -->
<ul id="post-list">
  {% for post in site.posts %}
    {% assign cat_slugs = post.categories | join: "," | split: "," | uniq %}
    <li data-categories="{{ cat_slugs | join: ' ' }}">
      <a href="{{ post.url }}">{{ post.title }}</a>
      <small>{{ post.date | date: "%b %-d, %Y" }}</small>
      <p>
        {% for category in post.categories %}
          <span>{{ category }}</span>{% unless forloop.last %}, {% endunless %}
        {% endfor %}
      </p>
    </li>
  {% endfor %}
</ul>

<!-- JavaScript filter logic -->
<script>
  const buttons = document.querySelectorAll('#category-buttons button');
  const posts = document.querySelectorAll('#post-list li');

  buttons.forEach(btn => {
    btn.addEventListener('click', () => {
      const cat = btn.dataset.category;
      posts.forEach(post => {
        const cats = post.dataset.categories.split(" ");
        post.style.display = (cat === "all" || cats.includes(cat)) ? "" : "none";
      });
    });
  });
</script>

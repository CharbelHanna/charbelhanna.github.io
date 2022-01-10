---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: single
excerpt: "my experience written and shared "
entries_layout: grid
author_profile: true
author: komo
classes: wide
header:
  show_overlay_excerpt: true
  #overlay_image: /assets/images/7.jpg
  overlay_filter: 0.5
  feature_image: true
related: true

---
<div class="grid__wrapper">
  {% for post in site.posts limit:4 %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
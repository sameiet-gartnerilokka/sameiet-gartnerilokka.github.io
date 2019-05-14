---
layout: archive
permalink: /
title: "Velkommen"
---
<div align="center">
<img src="{{ site.baseurl }}/images/overview_sameie.jpg" align="center" style="margin-bottom:10px">
</div>

Siste meldinger
<div class="tiles">
{% for post in site.posts %}
	{% include post-grid.html %}
{% endfor %}
</div><!-- /.tiles -->

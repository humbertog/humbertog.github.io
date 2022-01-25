---
layout: page
permalink: /publications_sci/
title: scientific publications
description: list of scientific publications in peer reviewed journals and conferences
years: [2021, 2020, 2019, 2018, 1935]
nav: false
---
<!-- _pages/publications_sci.md -->

<div class="publications">
<h2 class="typeofpub">PhD thesis</h2><br>

{%- for y in page.years %}
  {% bibliography -f thesis -q @*[year={{y}}]* %}
{% endfor %}


<h2 class="typeofpub">journal articles & conferences</h2><br><br>

{%- for y in page.years %}
  <!-- <h2 class="year">{{y}}</h2> -->
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>
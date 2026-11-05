---
layout: page
permalink: /publications/
title: publications
description: Papers are organized in reverse chronological order. Feel free to email me for full texts.
nav: true
nav_order: 6
---

<!-- _pages/publications.md -->
<div class="publications">

<!-- In-press entries are tagged `inpress = {true}` in papers.bib; they are listed here and excluded from the year groups below. -->
<h2 class="bibliography">In press</h2>
{% bibliography --query @*[inpress=true] --group_by none %}

{% bibliography --query @*[inpress!=true] %}

</div>

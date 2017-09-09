# A little bit about me...

I'm Kelly Mack, a student at the University of Illinois at Urbana-Champaign majoring in Computer Science with minors in Business and Mathematics. I love to learn and try new things.

<h2>{{ site.data.nav.docs_list_title }}</h2>
<ul>
   {% for item in site.data.nav.docs %}
      <li><a href="{{ item.url }}" alt="{{ item.title }}">{{ item.title }}</a></li>
   {% endfor %}
</ul>

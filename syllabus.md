---
layout: default
img: vintage_socialmedia
img_link: https://wronghands1.files.wordpress.com/2013/03/vintage-social-networking1.jpg
caption: Vintage Social Media
active_tab: syllabus
---

Subject to change as the term progresses. The lectures marked as NEW are for CSE 5539-0010 Fall 2017. The lectures marked as OLD are used at NASSLLI, which are to be updated and used in CSE 5539. 

<table class="table table-striped"> 
  <tbody>
    <tr>
      <th>Lecture</th>
      <th>Topic</th>
      <th>Readings</th>

    {% for lecture in site.data.syllabus %}
    <tr>
      <td>{{ lecture.date }}
      <td>
	{% if lecture.profile %}
	Company Profile:  
        {% endif %}
        {% if lecture.slides %}<a href="{{ lecture.slides }}">{{ lecture.title }}</a>
        {% else %}{{ lecture.title }}{% endif %}

	{% if lecture.speaker %}
        {% if lecture.speaker_url %} by <a href="{{ lecture.speaker_url }}">{{ lecture.speaker }}</a>
        {% else %} by {{ lecture.speaker }}{% endif %}
	{% endif %}

	{% if lecture.highlights %}
	  <ul>
	   {% for highlight in lecture.highlights %}	
	   <span class="text-muted"><li>
	   {{ highlight }}
	   </li></span>
          {% endfor %}
        {% endif %}
      <td>
        {% if lecture.reading %}
          <ul class="fa-ul">
          {% for reading in lecture.reading %}
            <li>
            {% if reading.optional %}<i class="fa-li fa fa-star"> </i>
            {% else %}<i class="fa-li fa"> </i> {% endif %}
            {% if reading.url %}
            <a href="{{ reading.url }}">{{ reading.title }}</a>
            {% else %}
            {{ reading.title }} 
            {% endif %}
	    {% if reading.author %}
            by {{ reading.author }}
            {% endif %}
            </li>
          {% endfor %}
          </ul>
        {% endif %} 
    {% endfor %}



## Publications

<br>

{% comment %} Counter for numbering in reverse chronological order. {% endcomment %}

{% assign index_counter = site.data.Publications | size %}

{% comment %} Group publications by year. {% endcomment %}

{% assign publication_groups = site.data.Publications | group_by: "year"  %}

{% for publication_group in publication_groups %}
  {% comment %} Process records into an array of HTML strings for output. {% endcomment %}

  {% assign publication_group_html = "" | split: ',' %}

  {% for record in publication_group.items %}
    {% comment %} Only process records that have at least a list of authors and a title. {% endcomment %}

    {% if record.authors and record.title %}
      {% comment %} Concatenate authors. {% endcomment %}

      {% for author in record.authors %}
        {% if forloop.first == true %}
          {% assign author_list = author %}
        {% elsif forloop.last == true %}
          {% assign author_list = author_list | append: " and " | append: author %}
        {% else %}
          {% assign author_list = author_list | append: ", " | append: author %}
        {% endif %}
      {% endfor %}

      {% comment %} Join author list and title. {% endcomment %}

      {% capture record_html %}{{ author_list }}, "{{ record.title }}"{% endcapture %}

      {% comment %} Add journal information. {% endcomment %}

      {% if record.journal %}
        {% capture record_html %}{{ record_html }}, <i>{{ record.journal }}</i>{% endcapture %}

        {% if record.volume %}
          {%- capture record_html %}{{ record_html }}, <b>{{ record.volume }}</b>{% endcapture %}
        {% endif %}

        {% if record.issue %}
          {%- capture record_html %}{{ record_html }} (<i>{{ record.issue }}</i>){% endcapture %}
        {% endif %}

        {% if record.article_number %}
          {%- capture record_html %}{{ record_html }}, {{ record.article_number }}{% endcapture %}
        {% elsif record.pages %}
          {% capture record_html %}{{ record_html }}, {{ record.pages }}{% endcapture %}
        {% endif %}

        {% if record.year %}
          {%- capture record_html -%}{{record_html}} (<b>{{record.year}}</b>){%- endcapture -%}
        {% endif %}
      {% endif %}

      {% comment %} Add a DOI and link if available. {% endcomment %}

      {% if record.doi %}
        {%- capture record_html -%}
          {{ record_html }}, DOI: <a href="https://doi.org/{{ record.doi }}">{{ record.doi }}</a>
        {%- endcapture -%}
      {% endif %}

      {% comment %} Add an arXiv number and link if available. {% endcomment %}

      {% if record.arxiv %}
        {%- capture record_html -%}
          {{record_html}}, arXiv: <a href="https://arxiv.org/abs/{{record.arxiv}}">{{record.arxiv}}</a>
        {%- endcapture -%}
      {% endif %}

      {% comment %} Store record HTML. {% endcomment %}

      {% assign publication_group_html = publication_group_html | push: record_html %}
    {% endif %}
  {% endfor %}

{% comment %} Output to page. {% endcomment %}

<p><b>Publications in {{ publication_group.name }}</b></p>
<hr>

{%- for record_html in publication_group_html %}
  <p>{{ index_counter }}. {{ record_html }}</p>
  {% assign index_counter = index_counter | minus: 1 %}
{%- endfor %}

{% if forloop.last == false %}<br>{% endif %}

{% endfor %}

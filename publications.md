---
title: Publications

publication_types:
  - 'preprint'
  - 'journal-article'
  - 'conference-proceedings'
  - 'book-chapter'
---

{% comment %}
  The content on this page is generated from a list of records in _data/Publications.yml.
  This block of Liquid code groups the records by year, selects records of the types specified in the page front matter, and converts them to HTML snippets that can be styled into a list.
  The results are stored in a collection containing, for each year, a title string plus snippets for each type of publication, which can then be used to build the page content.
  It also keeps a count of the total number of records, which can be used for numbering the list across the year grouping.
  The parsed data is then output into the page content in another, smaller Liquid block.
  Since Liquid is somewhat clunky, separating the logic into preparation and output (a) makes the code easier to follow, and (b) makes it easier to adjust the styling without touching the YAML -> HTML code.
{% endcomment %}

{% comment %}
  Variables to capture:
    parsed_record_groups : a set of arrays (one per block) structured as: [ year, [ [ type_1_snippet_1, ..., type_1_snippet_N ], ..., [ type_N_snippet_1, ... ] ] ]
    parsed_records_total : total number of records to be output, used to number publications in reverse order across groups.
{% endcomment %}

{% assign parsed_record_groups = "" | split: ',' %}
{% assign parsed_records_total = 0 %}

{% comment %} Group publications by year. {% endcomment %}

{% assign record_groups = site.data.publications | sort : "year" | reverse | group_by: "year"  %}

{% for record_group in record_groups %}
    {% comment %} Title string. {% endcomment %}

    {% capture title_str %}Publications in {{ record_group.name }}{% endcapture %}

    {% comment %} Groups of list items. {% endcomment %}

    {% assign list_item_groups = "" | split: ',' %}

    {% for publication_type in page.publication_types %}
        {% comment %} Select records of current publication type. {% endcomment %}

        {% assign record_group_filtered = record_group.items | where: "type", publication_type %}

        {% comment %} Process records and collect list items. {% endcomment %}

        {% assign list_item_group = "" | split: ',' %}

        {% for record in record_group_filtered %}
            {% comment %} Only process records that have at least a list of authors and a title. {% endcomment %}

            {% if record.authors and record.title %}
                {% comment %} Concatenate authors. {% endcomment %}

                {% for author in record.authors %}
                    {% if forloop.first %}
                        {% assign author_list = author %}
                    {% elsif forloop.last %}
                        {% assign author_list = author_list | append: " and " | append: author %}
                    {% else %}
                        {% assign author_list = author_list | append: ", " | append: author %}
                    {% endif %}
                {% endfor %}

                {% comment %} Join author list and title. {% endcomment %}

                {% capture list_item %}{{ author_list }}, "{{ record.title }}"{% endcapture %}

                {% comment %} Add publication information if available. {% endcomment %}

                {% if publication_type == 'book-chapter' %}
                    {% comment %} Book information. {% endcomment %}

                    {% if record.book_title %}
                        {% capture list_item %}{{ list_item }} in {% endcapture %}

                        {% if record.editors %}
                            {% for editor in record.editors %}
                                {% if forloop.first %}
                                    {% assign editor_list = editor %}
                                {% elsif forloop.last %}
                                    {% assign editor_list = editor_list | append: " and " | append: editor %}
                                {% else %}
                                    {% assign editor_list = editor_list | append: ", " | append: editor %}
                                {% endif %}
                            {% endfor %}

                            {% capture list_item %}{{ list_item }} {{editor_list}} (eds.), {% endcapture %}
                        {% endif %}

                        {% capture list_item %}{{ list_item }} "{{ record.book_title }}"{% endcapture %}

                        {% if record.series %}
                            {% capture list_item %}{{ list_item }}, <i>{{ record.series }}</i>{% endcapture %}

                            {% if record.volume %}
                                {% capture list_item %}{{list_item}} <b>{{ record.volume }}</b>{% endcapture %}
                            {% endif %}
                        {% endif %}
                    {% endif %}

                {% else %}
                    {% comment %} Journal information. {% endcomment %}

                    {% if record.journal %}
                        {% capture list_item %}{{ list_item }}, <i>{{ record.journal }}</i>{% endcapture %}

                        {% if record.volume %}
                            {%- capture list_item %}{{ list_item }} <b>{{ record.volume }}</b>{% endcapture %}
                        {% endif %}

                        {% if record.issue %}
                            {% capture list_item %}{{ list_item }} (<i>{{ record.issue }}</i>){% endcapture %}
                        {% endif %}

                        {% if record.article_number %}
                            {%- capture list_item %}{{ list_item }}, {{ record.article_number }}{% endcapture %}
                        {% elsif record.pages %}
                            {% capture list_item %}{{ list_item }}, {{ record.pages }}{% endcapture %}
                        {% endif %}
                    {% endif %}

                {% endif %}

                {% comment %} Add year if available. {% endcomment %}

                {% if record.year %}
                    {% capture list_item %}{{ list_item }} (<b>{{ record.year }}</b>){% endcapture %}
                {% endif %}

                {% comment %} For book chapters, insert the publisher name and place of publication if available. {% endcomment %}

                {% if publication_type == 'book-chapter' %}
                    {% if record.publisher %}
                        {% capture list_item %}{{ list_item }}, {{ record.publisher }}{% endcapture %}

                        {% if record.publication_place %}
                            {% capture list_item %}{{ list_item }}: {{ record.publication_place }}{% endcapture %}
                        {% endif %}
                    {% endif %}
                {% endif %}

                {% comment %} Add a DOI and link if available. {% endcomment %}

                {% if record.doi %}
                    {%- capture list_item %}
                        {{ list_item }}, DOI: <a href="https://doi.org/{{ record.doi }}" target="_blank">{{ record.doi }}</a>
                    {%- endcapture %}
                {% endif %}

                {% comment %} For book chapters, add ISBN(s) if available. {% endcomment %}

                {% if publication_type == 'book-chapter' %}
                    {% if record.isbn %}
                        {% for isbn in record.isbn %}
                            {% if forloop.first %}
                                {% capture isbn_string %}{{ isbn.number }} ({{ isbn.type }}){% endcapture %}
                            {% else %}
                                {% capture isbn_string %}{{ isbn_string }}, {{ isbn.number }} ({{ isbn.type }}){% endcapture %}
                            {% endif %}
                        {% endfor %}

                        {% capture list_item %}{{ list_item }}, ISBN: {{ isbn_string }}{% endcapture %}
                    {% endif %}
                {% endif %}

                {% comment %} Add links to preprints if available. {% endcomment %}

                {% if record.arxiv %}
                    {%- capture list_item %}
                        {{ list_item }}, arXiv: <a href="https://arxiv.org/abs/{{record.arxiv}}" target="_blank">{{ record.arxiv }}</a>
                    {%- endcapture %}
                {% endif %}

                {% comment %} Strip whitespace and add list item to group. {% endcomment %}

                {% assign list_item = list_item | strip %}

                {% assign list_item_group = list_item_group | push: list_item %}
            {% endif %}
        {% endfor %}

        {% comment %} Store list item group. {% endcomment %}

        {% assign list_item_groups = list_item_groups | push: list_item_group %}
    {% endfor %}

    {% comment %} Store title string and groups of list items. {% endcomment %}

    {% assign parsed_record_group = "" | split: ',' %}
    {% assign parsed_record_group = parsed_record_group | push: title_str %}
    {% assign parsed_record_group = parsed_record_group | push: list_item_groups %}

    {% assign parsed_record_groups = parsed_record_groups | push: parsed_record_group %}

    {% comment %} Update record count. {% endcomment %}

    {% assign parsed_record_count = 0 %}

    {% for list_item_group in list_item_groups %}
        {% assign parsed_record_count = parsed_record_count | plus: list_item_group.size %}
    {% endfor %}

    {% assign parsed_records_total = parsed_records_total | plus: parsed_record_count %}
{% endfor %}

## Publications
---------------

<br>

{% comment %} Build page content. {% endcomment %}

{% assign output_counter = parsed_records_total %}

{% for record_group in parsed_record_groups %}

<h3>{{ record_group[0] }}</h3>
<hr>

{% for list_item_group in record_group[1] %}

{% unless forloop.first %}
  {% assign index_prev = forloop.index0 | minus: 1 %}

  {% if list_item_group.size > 0 and record_group[1][index_prev].size > 0 %}<hr>{% endif %}
{% endunless %}

{% for list_item in list_item_group %}

<p>{{ output_counter }}. {{ list_item }}</p>

{% assign output_counter = output_counter | minus: 1 %}

{% endfor %}
{% endfor %}

{% unless forloop.last %}<br>{% endunless %}

{% endfor %}

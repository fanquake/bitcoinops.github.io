---
title: Topics
permalink: /en/topic-dates/
layout: page
---
{% include linkers/topic-pages.md %}
<div>{% comment %}<!-- enclosing in a div forces this to be interpreted
as HTML rather than Markdown so indentation over 4 characters doesn't
produce code blocks -->{% endcomment %}

{% capture raw_mentions %}
{%- for topic in site.topics -%}
  {%- for mention in topic.optech_mentions -%}
    {{mention.date}}DIVIDER<a class="pc125" href="{{mention.url}}">🔗</a>DIVIDER{{mention.title}}DIVIDER<a href="{{topic.url}}">{{topic.title}}</a>ENDMENTION
  {%- endfor -%}
{%- endfor -%}
{% endcapture %}
{% assign mentions = raw_mentions | split: 'ENDMENTION' | sort | reverse %}

{%- for mention in mentions -%}
  {%- assign mention_part = mention | split: "DIVIDER" -%}
  {%- assign mention_date = mention_part[0] -%}
  {%- assign mention_link = mention_part[1] -%}
  {%- assign mention_title = mention_part[2] -%}
  {%- assign mention_topic = mention_part[3] -%}

  {%- if mention_link == old_mention_link -%}
    {%- assign combined_topics = combined_topics | append: ', ' | append: mention_topic -%}
    {%- capture item -%}<li><p>{{mention_title}}&nbsp;{{mention_link}}<br>{{combined_topics}}</p></li>{%- endcapture -%}
  {%- else -%}
    {%- comment -%}<!-- New URL, new item - so output old item and reset topic collector -->{%- endcomment -%}
    {{item}}
    {%- assign combined_topics = mention_topic -%}
    {%- capture item -%}<li><p>{{mention_title}}&nbsp;{{mention_link}}<br>{{mention_topic}}</p></li>{%- endcapture -%}
  {%- endif -%}

  {%- assign year_month = mention_date | truncate: 7, '' -%}
  {%- if year_month != lastym -%}
    {% if lastym != nil %}</ul>{% endif %}
    <h3>{{mention_date | date: '%B %Y'}}</h3>
    <ul>
  {%- endif -%}

  {% assign old_mention_link = mention_link %}
  {% assign lastym = year_month %}
{% endfor %}
{% comment %}<!-- The loop doesn't display a mention until the next
mention is seen, so we will always have an undisplayed mention at the
end of the loop.  Display it now.  Also close our final list -->{% endcomment %}
{{item}}
</ul>

</div>

{% include linkers/request-a-topic.md %}

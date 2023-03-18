---
layout: default
---
{%- include multi_lng/get-pages-by-lng.liquid pages = site.posts -%}

<div class="multipurpose-container home-heading-container">
  <div class="home-heading" style="background-image:url('{{ page.img }}');">
    <div class="home-heading-message">
      {{ site.data.lang[lng].home.top_header_line1 | replace: site.data.conf.main.brand_replace, site.data.owner.brand }}
      {%- if site.data.lang[lng].home.top_header_line2 %}
        <br>
        {{ site.data.lang[lng].home.top_header_line2 | replace: site.data.conf.main.brand_replace, site.data.owner.brand }}
      {% endif -%}
    </div>
  </div>
  <div class="home-intro-text">
    {{ content }}
  </div>
</div>
{% if page.faq %}
{% include accordion.html %}
{% endif %}
{%- if lng_pages.size > 0 and site.data.conf.others.home.new_posts %}
<div class="multipurpose-container new-posts-container">
  <div>{{ site.data.lang[lng].home.new_posts_title }}</div>
  <ul class="new-posts">
  {%- for _post in lng_pages limit: site.data.conf.others.home.new_posts_count_limit -%}
    <li>
      {%- assign page_title = _post.title -%}
      {%- include util/auto-content-post-title-rename.liquid title = page_title -%}
      <a href="{{ site.baseurl }}{{ _post.url }}">{{ page_title }}
        <span>{{ _post.date | date_to_string | date: site.data.lang[lng].date.long }}</span>
      </a>
    </li>
  {% endfor -%}
    <li>
      {%- include multi_lng/get-page-by-layout.liquid layout = 'archives' -%}
      <a href="{{ site.baseurl }}{{ layout_page_obj.url }}">{{ site.data.lang[lng].home.new_posts_show_more_button }}</a>
    </li>
  </ul>
</div>
{% endif -%}
{%- if site.data.conf.posts.post_table_of_contents_tooltip -%}
  {%- capture tooltip_move -%} data-toggle="tooltip" data-placement="top" title="{{ site.data.lang[lng].post.table_of_contents.tooltip.move }}" {%- endcapture %}
  {%- capture tooltip_close -%} data-toggle="tooltip" data-placement="bottom" title="{{ site.data.lang[lng].post.table_of_contents.tooltip.close }}" {%- endcapture %}
{%- endif -%}
<div id="toc-container" class="movable">
  <div class="panel panel-default">
    <div class="panel-heading" {{ tooltip_move }}>
      {{ site.data.lang[lng].post.table_of_contents.heading }}
      <span class="pull-right">
        {%comment-%} close button {%endcomment-%}
        <a href="javascript:void(0);" class="close-button" onclick="document.getElementById('toc-container').style.display = 'none';">
          <i class="fa fa-times" {{ tooltip_close }}></i>
        </a>
      </span>
    </div>
    <div class="panel-body">
      <nav id="table-of-contents"></nav>
    </div>
  </div>
</div>



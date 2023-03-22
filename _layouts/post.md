---
layout: default
---
{%- include multi_lng/get-lng-by-url.liquid -%}
{%- assign lng = get_lng -%}
{%- include post_common/post-main.html post = page -%}
{% if page.faq %}
{% include accordion.html %}
{% endif %}
{%-comment-%} Pagination {%-endcomment-%}
{% if site.posts.size > 1 -%}
  {% include multi_lng/get-pages-by-lng.liquid pages = site.posts -%}
  {% if site.data.conf.posts.pager_navigation_post == 'prev_next_buttons' -%}
    {%- include post_common/pager-prev-next-buttons.html pages = lng_pages current_page_url = page.url side_aligned = site.data.conf.posts.pager_prev_next_buttons_side_aligned -%}
  {% elsif site.data.conf.posts.pager_navigation_post == 'page_numbers' %}
    {% include post_common/pager-page-numbers.html pages = lng_pages current_page_url = page.url -%}
  {% endif -%}
{% endif -%}

{% if site.data.conf.posts.comments.enable and site.data.conf.posts.comments.disqus.enable and page.comments_disable != true %}
  {% include post/disqus.html %}
{% endif %}



<!-- Comments -->
{% if site.data.comments[page.slug] %}
    <h3>
    {% if site.data.comments[page.slug].size > 1 %}
      {{ site.data.comments[page.slug] | size }}
    {% endif %}
    Comments:
    </h3>
  {% assign comments = site.data.comments[page.slug] | sort %}
    {% for comment in comments %}
      <label>
        {% if comment[1].url %}
          <a href="{{ comment[1].url }}">
        {% endif %}
        <strong>{{ comment[1].name }}</strong>
        {% if comment[1].url %}
          </a>
        {% endif %}
      </label>
      <em>{{ comment[1].date | date: "%B %d, %Y" }}</em>
      <p>{{ comment[1].message | markdownify }}</p>
    {% endfor %}
{% endif %}
<!-- Comments Form -->
  <form method="POST" action="{{ site.staticman_url }}">
    <input name="options[redirect]" type="hidden" value="https://culture.irangerai.com">
    <input name="options[slug]" type="hidden" value="{{ page.slug }}">
      <label>Name</label>
      <input name="fields[name]" type="text">
      <label>E-mail (optional)</label>
      <input name="fields[email]" type="email">
      <label>Website (optional)</label>
      <input name="fields[url]" type="url">
      <label>Message</label>
      <textarea style="width:100%" name="fields[message]" rows="12"></textarea>
      <small>Comments will appear after moderation.</small>
      <button type="submit">Submit comment</button>
  </form>

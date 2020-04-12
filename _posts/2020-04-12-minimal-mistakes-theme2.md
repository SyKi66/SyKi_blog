---
layout: single

title: "minimal mistake theme 지킬 블로그 커스터마이징하기"
# excerpt: ""

date: 2020-04-12 13:50:00 +0900
lastmod: 2020-04-12 13:50:00 +0900

author_profile: false

toc: true
toc_label: "Contents"
toc_icon: "bars"
toc_sticky: true

header:
  overlay_image: https://images.unsplash.com/photo-1501785888041-af3ef285b470?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80
  overlay_filter: 0.5

categories: 
  - blog

tags: 
    - blog
    - theme
    - customize
    - jekyll

show_overlay_excerpt: false
---

추가적으로 커스터마이징하기

# 1. footer 부분의 powered by 문구 삭제하기

_includes 에서 footer.html 의 하단부분을 아래와 같이 바꾸어주면 powered by 문구가 사라진다.

```html
<div class="page__footer-copyright">&copy; {{ site.time | date: '%Y' }} {{ site.name | default: site.title }}.</div>
```

<br/>

---

# 2. 헤더에 이미지 추가 및 높이 조정

_include/masthead.html을 아래와 같이 변경하면되며(random_BG nav에 투명 스타일 추가, 맨아래 스크립트 추가), 방법은 링크 참조

반드시 script 는 가장 아래에 넣어야된다.

```html
{% raw %}
{% capture logo_path %}{{ site.logo }}{% endcapture %}

<div class="masthead ">
  <div class="masthead__inner-wrap random_BG">
    <div class="masthead__menu">
      <nav id="site-nav" class="greedy-nav" style="background-color: transparent;">
        {% unless logo_path == empty %}
          <a class="site-logo" href="{{ '/' | relative_url }}"><img src="{{ logo_path | relative_url }}" alt=""></a>
        {% endunless %}
        <a class="site-title" href="{{ '/' | relative_url }}">
          {{ site.masthead_title | default: site.title }}
          {% if site.subtitle %}<span class="site-subtitle">{{ site.subtitle }}</span>{% endif %}
        </a>
        <ul class="visible-links">
          {%- for link in site.data.navigation.main -%}
            <li class="masthead__menu-item">
              <a href="{{ link.url | relative_url }}"{% if link.description %} title="{{ link.description }}"{% endif %}>{{ link.title }}</a>
            </li>
          {%- endfor -%}
        </ul>
        {% if site.search == true %}
        <button class="search__toggle" type="button">
          <span class="visually-hidden">{{ site.data.ui-text[site.locale].search_label | default: "Toggle search" }}</span>
          <svg class="icon" width="16" height="16" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 15.99 16">
            <path d="M15.5,13.12L13.19,10.8a1.69,1.69,0,0,0-1.28-.55l-0.06-.06A6.5,6.5,0,0,0,5.77,0,6.5,6.5,0,0,0,2.46,11.59a6.47,6.47,0,0,0,7.74.26l0.05,0.05a1.65,1.65,0,0,0,.5,1.24l2.38,2.38A1.68,1.68,0,0,0,15.5,13.12ZM6.4,2A4.41,4.41,0,1,1,2,6.4,4.43,4.43,0,0,1,6.4,2Z" transform="translate(-.01)"></path>
          </svg>
        </button>
        {% endif %}
        <button class="greedy-nav__toggle hidden" type="button">
          <span class="visually-hidden">{{ site.data.ui-text[site.locale].menu_label | default: "Toggle menu" }}</span>
          <div class="navicon"></div>
        </button>
        <ul class="hidden-links hidden"></ul>
      </nav>
    </div>
  </div>
</div>

<script src="{{'/assets/customJS/randomBackgroundImage.js' | relative_url }}"></script>
{% endraw %}
```




# 2. 발췌부분



# 1. favicon 추가

# 2. 블록지정 색 변경

# 3. footer powerby 변경

# 4. header 이미지 높이 변경

# 5. archive 발췌 없애기

# 6. 밑줄제거


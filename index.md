## メモ置き場

メモした内容と保存場所を頻繁に忘れるのでここに集約する

## Latest posts

{% for post in site.posts limit: 5 %}
- [{{ post.title }}]({{ post.url | prepend: site.baseurl }})
{% endfor %}

## カテゴリ
{% for category in site.categories %}
### {{ category[0] }}
{% for post in category[1] %}
- [{{ post.title }}]({{ post.url | prepend: site.baseurl }})
{% endfor %}
{% endfor %}


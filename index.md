## メモ置き場

メモした内容と保存場所を頻繁に忘れるのでここに集約する

## Latest posts

{% for post in site.posts limit: 10 %}
- [{{ post.title }}]({{ post.url | prepend: site.baseurl }})
{% endfor %}

## カテゴリ
{% for category in site.categories %}
- {{ category[0] }}
{% endfor %}

### メモリスト

[vscodeでMarkdown](articles/markdown.md)  
[rosでcatkinコマンド](articles/ros_catkin.md)  
[vscodeの標準・拡張機能のメモ](articles/vscode.md)  
[WSL](articles/wsl.md)  
[3DLidar(VLP16)のセットアップ](articles/velodyne_VLP16_setup_memo.md)  

## メモ置き場

メモした内容と保存場所を頻繁に忘れるのでここに集約する

## Latest posts

{% for post in site.posts limit: 3 %}
- [{{ post.title }}]({{ post.url | prepend: site.baseurl }})
{% endfor %}

### メモリスト

[Gitの初歩](articles/git.md)  
[vscodeでMarkdown](articles/markdown.md)  
[rosでcatkinコマンド](articles/ros_catkin.md)  
[GitHub Actionsの使い方](articles/github_actions.md)  
[vscodeの標準・拡張機能のメモ](articles/vscode.md)  
[WSL](articles/wsl.md)  
[3DLidar(VLP16)のセットアップ](articles/velodyne_VLP16_setup_memo.md)  
[url link](articles/link.html)  

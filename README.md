# memo
様々なツールの使い方のメモ書き

https://tomson784.github.io/memo/

## site preview

ローカルで試したい場合は以下のコマンドを実行する．
```
docker run -it --rm -v "$(pwd):/usr/src/app" -p "4000:4000" starefossen/github-pages
```
[localhost:4000](http://localhost:4000)にアクセスすると表示される．

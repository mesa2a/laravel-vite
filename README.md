# Dockerを使用したLaravel10の環境構築 

dockerフォルダ内にmysqlフォルダを作成してください。
コンテナを立ち上げます。

```
docker compose up -d
```

appサーバのコンテナに入り、コマンドを入力します。

```
$ cd /src
$ mkdir ltmp
$ cd ltmp
$ composer create-project "laravel/laravel=10.*" . --prefer-dist
$ cd /src
$ mv ltmp/* ./
$ mv ltmp/.* ./
$ rm ltmp -rf
$ cd /src
$ composer install
$ npm install
chmod -R guo+w storage
$ php artisan storage:link
$ npm run dev
```

http://localhost/ でLaravelのデフォルトのページにアクセスできたらLaravelの導入は成功です。
次は、Viteの設定を行います。
welcome.blade.phpに`@vite`文を追記します。
```html
<!DOCTYPE html>
<html ...>
    <head>
       //省略
 
        @vite(['resources/css/app.css', 'resources/js/app.js'])
    </head>
```
vite.config.jsを以下のように変更します。
```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';

export default defineConfig({
    plugins: [
        laravel({
            input: ['resources/css/app.css', 'resources/js/app.js'],
            refresh: true,
        }),
    ],
    server: {
        host: true,
        hmr: {
            host: 'localhost',
        },
        watch: {
            usePolling: true,
          },
    },
});
```
読み込んでいるCSSを編集し、瞬時に反映されれば環境構築は完了です。
app.css
```css
.text-xl{
    color: blue;
}
```

## 参考
- [Laravel 10 の開発環境をdockerで実現する方法](https://qiita.com/hitotch/items/869070c3a9f474a358ea)

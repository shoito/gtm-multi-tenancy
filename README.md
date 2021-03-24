# Google Tag Managerを使ってテナントUIを試す

ヘッダー部分をテナント毎にUIを異なるものにする例の検証コード。

## 実装

src/main/resources/templates/hello.html のようにGoogle Tag Managerのタグをガイドに従って挿入しておく。  
テナント毎にカスタマイズしたいUI部分をGoogle Tag ManagerのカスタムHTMLから操作しやすいようにid属性を付与しておく。

```html
<!doctype html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8" />
    <title>Hello</title>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
    new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
    j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
    'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
    })(window,document,'script','dataLayer','GTM-XXX');</script>
    <!-- End Google Tag Manager -->
</head>
<body>
<!-- Google Tag Manager (noscript) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-XXX"
                  height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
<!-- End Google Tag Manager (noscript) -->

<header id="tenant-header"><!-- テナント毎ヘッダー --></header>
<h1 th:text="${message}"></h1>
<p>共通部分</p>
<footer id="tenant-footer"><!-- テナント毎フッター --></footer>
</body>
</html>
```

## Google Tag ManagerでテナントUIのタグを設定

1. Google Tag Managerに検証用のアカウントを作る
1. テナントAのタグとしてカスタムHTMLを作る
   ```html
    <script>
    document.getElementById("tenant-header").innerHTML = "テナントAのヘッダー";
    </script>
    ```
1. テナントAの上記タグを挿入するためのトリガーを作る
   - トリガーのタイプ
      - ページビュー - DOM Ready
   - このトリガーの発生場所
      - 一部の DOM Ready イベント
      - Page Hostname: tenant-a.local
      - Page Path: /hello
1. テナント数分、上記を繰り返す
1. プレビューで実際にテナントUIのタグが挿入されることを確認する

## 確認方法

1. `/etc/hosts` にテナント分のサブドメインを設定する
    ```shell
    127.0.0.1 tenant-a.local
    127.0.0.1 tenant-b.local
    ...
    ```   

1. プログラムを実行する
    ```shell
    ./gradlew bootRun
    ```

1. ブラウザから各テナントのURLにアクセスする
    `http://tenant-a.local:8080/hello`
    `http://tenant-b.local:8080/hello`

## Google Tag Managerの参考画面
![](https://user-images.githubusercontent.com/37051/112310787-4f8eec00-8ce8-11eb-9b90-b48100170f2f.png)
![](https://user-images.githubusercontent.com/37051/112310794-51f14600-8ce8-11eb-8373-becafd15582c.png)
![](https://user-images.githubusercontent.com/37051/112310809-5584cd00-8ce8-11eb-9193-1e17d50cbd22.png)
![](https://user-images.githubusercontent.com/37051/112310815-57e72700-8ce8-11eb-99f8-45c415f070d0.png)
![](https://user-images.githubusercontent.com/37051/112310820-59185400-8ce8-11eb-9785-05144a3a4381.png)
![](https://user-images.githubusercontent.com/37051/112310830-5ae21780-8ce8-11eb-8586-28a8047a3b7e.png)
![](https://user-images.githubusercontent.com/37051/112310835-5cabdb00-8ce8-11eb-942c-9fbf1110e9bd.png)
![](https://user-images.githubusercontent.com/37051/112310846-5fa6cb80-8ce8-11eb-8adf-a0a90727454f.png)
![](https://user-images.githubusercontent.com/37051/112310848-5fa6cb80-8ce8-11eb-84fe-5c9369b5d55f.png)

# Rust 向け拡張課題

## 要件

### 機能要件

以下のように動作する API エンドポイントを実装してください．すべての都道府県に対する総人口と年少人口の統計を返す API となります．

```bash
$ curl -sSL https://localhost:8000/api/v1/populations | jq
{
  "北海道": [
    "years": {
      "1980": {
        "all": 12345, # 総人口
        "children": 6789 # 年少人口
      },
      # ...
    }
  ],
  # ...
}
```

- 表示のわかりやすさのため pretty print した値を表示 (`| jq`) しています．サーバの返す文字列は minify されたものでもかまいません
- 一部省略して記述しています．コメントもわかりやすさのために記述しているので，この通りに返す必要はまったくありません．
- 数値は仮の値です．実際に取得した値が表示されるようにしてください
- データは [RESAS API](https://opendata.resas-portal.go.jp/) から取得できる
  [都道府県一覧](https://opendata.resas-portal.go.jp/docs/api/v1/prefectures.html) および
  [人口構成](https://opendata.resas-portal.go.jp/docs/api/v1/population/composition/perYear.html) を利用してください．
  ただし，表示する人口は総人口および年少人口の値とし，都道府県単位で取得してください．
- 自己署名用証明書を作成し，それを使って HTTP/2 over TLS 1.3 でリクエストを受け付けられるようにしてください．
  HTTP 通信からのリダイレクトや HSTS，Alt-Svc 等の設定は任意です．

### 非機能要件

- hyper, actix-web, axum のいずれかを使って Web アプリを構築してください
- 外部の API (適当なオープンデータ) からデータを取得するサービスを実装してください
    - ただし，これは非同期で実行されるようにしてください (非同期ランタイムは任意) ．
    - ただし，ユースケースからは抽象にのみ依存するようにしてください
    - 具象の 1 つとしてモックを実装し，ユニットテストでは外部へのアクセスなしでロジックを確認してください
- エンドポイントを 1 つ作成し，先述したサービスを用いてデータを取得するようにしてください
    - ただし，コントローラとユースケースは分離してください
- JWT を検証することによる認証機能を実装してください
    - ただし，これはコントローラとは分離して，それぞれのフレームワークにおけるミドルウェア (または Interceptor) のしくみを利用してください
    - 鍵の生成などを行う必要はありませんので，こちらであらかじめ用意した鍵と JWT トークンを利用してください
- いくらかのテストを追加してください．
    - サービスとユースケースにはユニットテストを追加してください
    - API の動作を確認する E2E テストを追加してください
        - この E2E テストについては，実際に RESAS API へアクセスするようにしてください
- (オプション) 任意の DI ライブラリを利用するか， DI コンテナを自作して具象を注入してください
- (オプション) 制作の過程はローカルの Git リポジトリで管理し， .git ディレクトリを残したまま提出してください

## 注意事項

- すべての要件を満たすことができなくても評価は行えますので，そのまま提出してください
- MSRV (Minimum Supported Rust Version) は 1.70.0 としてください．こちらでの動作確認時も 1.70.0 で行います．
- サードパーティ製ライブラリを利用できます．
  ただし，ライブラリの利用条件について問題ないことを確認のうえ，外部のライブラリであることがわかる使い方をしてください．
  たとえば， Cargo の依存として追加することや， Git Submodule として追加することが挙げられます．
  どうしてもこれらの方法が使えないときは，コードを直接コピーして利用 (Vendoring) してもよいですが，入手元を記述し，どのファイルがそれに当たるかを明示してください．
- RESAS API の利用には API キーの発行が必要ですので，ご自身で行っていただきますようにお願いします．

## 評価ポイント

- 機能要件・非機能要件を満たしているか
- オプションの要件を満たしているか．満たしていなくても減点にはなりませんが，加点とはなります
- 適切にコードが分離・整理され，読みやすいような命名・構造となっているか．理由なく認知負荷の高いコードや重複の多いコードとなっていないか
- Rust の言語機能や標準ライブラリ，エコシステムを適切に理解し，利用できているか
- オプションとして Git リポジトリを提出した場合は，適切なコミットの粒度やメッセージが用いられているか

## お問い合わせ

問題について質問がある場合は，採用担当を通じて，または問題作成者に直接お問い合わせください．

```
Naoki Ikeguchi <n_ikeguchi@yumemi.co.jp>

-----BEGIN PGP PUBLIC KEY BLOCK-----

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
26Yid+vTyStuR8/qME4LMDt6T+bGOjRD6Nn2DA==
=0uot
-----END PGP PUBLIC KEY BLOCK-----
```

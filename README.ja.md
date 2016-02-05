org-redmine
==============================

[![Build Status](https://travis-ci.org/gongo/org-redmine.svg?branch=master)](https://travis-ci.org/gongo/org-redmine)

概要
--------------------

インストール
--------------------

### git でもってくる

1. org-redmine リポジトリを clone します

    ```
    $ git clone git://github.com/gongo/org-redmine.git
    ```

2. 以下を .emacs など、適当な箇所に書いておきます

    ```lisp
    (add-to-list 'load-path "/path/to/org-redmine/")
    (require 'org-redmine)
    ```

### auto-install ユーザ向け

以下を評価するとインストールできます

```lisp
(auto-install-from-url "https://raw.github.com/gongo/org-redmine/master/org-redmine.el")

;; もしくは、以下を実行してください
;; M-x auto-install-from-url RET https://raw.github.com/gongo/org-redmine/master/org-redmine.el
```

設定
--------------------

### URL

```lisp
;; 対象の Redmine の URL を設定します
;;   例. Redmine Project
(setq org-redmine-uri "http://www.redmine.org")
;;   例. Ruby Project
(setq org-redmine-uri "http://redmine.ruby-lang.org")
```

### 認証 (必須ではない)

認証設定は3つあります。優先度の高い順に紹介していきます。

すべて設定されていても、優先度の高いものだけ適用されます。

1. API Key

    REST API Key が設定可能です。デフォルトは nil になっています

    ```lisp
    (setq org-redmine-auth-api-key "xxxxxxxxxxxxxxxxxxxx")
    ```

2. ユーザ/パスワード認証

    ログインに用いるユーザ及びパスワードを使って認証を行います。

    ```lisp
    (setq org-redmine-auth-username "gongo")
    (setq org-redmine-auth-password "secret")
    ```

3. netrc 認証

    `$HOME/.netrc` を使って認証を行うかどうかを設定します。

    ```lisp
    (setq org-redmine-auth-netrc-use t) ;; デフォルトは nil
    ```

### テンプレートに用いるシーケンス一覧

| `%-sequence | mean                                 |
|-------------|--------------------------------------|
| `%as_i%`    | 担当者ID                             |
| `%as_n%`    | 担当者名                             |
| `%au_i%`    | チケット作成者ID                     |
| `%au_n%`    | チケット作成者名                     |
| `%c_i%`     | カテゴリID                           |
| `%c_n%`     | カテゴリ名                           |
| `%c_date%`  | チケット作成日                       |
| `%d%`       | 説明                                 |
| `%done%`    | 進捗率                               |
| `%d_date%`  | 期日                                 |
| `%i%`       | チケットID                           |
| `%pr_i%`    | 優先度ID                             |
| `%pr_n%`    | 優先度名(Normal とか重要とか)        |
| `%p_i%`     | プロジェクトID                       |
| `%p_n%`     | プロジェクト名                       |
| `%s_date%`  | 開始日                               |
| `%s_i%`     | ステータスID                         |
| `%s_n%`     | ステータス名 (Assigned とか完了とか) |
| `%s%`       | チケットのタイトル                   |
| `%t_i%`     | トラッカーID                         |
| `%t_n%`     | トラッカー名                         |
| `%u_date%`  | 更新日                               |
| `%v_n%`     | バージョン名                         |
| `%v_i%`     | バージョンID                         |

### 挿入するサブツリーのテンプレート

```lisp
;; デフォルトのテンプレート
;; (defvar org-redmine-template-header "#%i% %s% :%t_n%:")
;; (defvar org-redmine-template-property nil)

;; このように挿入されます
;; * [#333] Subject :Tag:

(setq org-redmine-template-header "[%p_n%] #%i% %s% by %as_n%")
(setq org-redmine-template-property
      '(("担当者" . "%as_n%")
        ("対象バージョン" . "%v_n%")))

;; このように挿入されます
;; * [ProjectName] #333 Subject by gongo
;;   :PROPERTIES:
;;   :担当者:  dududu
;;   :対象バージョン: 1.2
;;   :END:

(setq org-redmine-template-header "[#%i%] %s%")
(setq org-redmine-template-property
      '(("プロジェクト名" . "%as_n%")))

;; * [#333] Subject
;;   :PROPERTIES:
;;   :プロジェクト名:  ProjectName
;;   :END:
```

テンプレートで使用できるフォーマットは、org-redmine.el に載っています。

** ライセンスについて

MIT
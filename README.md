# .github

株式会社 Gizumo の GitHub Organization 共通設定を管理するリポジトリです。

このドキュメントは **Organization の設定を管理する運用者向け** の説明です。Organization の対外的な紹介文は `profile/README.md`（後述）が担当します。

## ⚠️ はじめに読むこと

`.github` は GitHub の **特殊リポジトリ** です。ここに配置したファイルは、Organization 配下のリポジトリに **デフォルト設定として自動的に継承** されます。

**このリポジトリへの変更は、Gizumo の全リポジトリに波及します。** 変更の前に、影響範囲を必ず確認してください。

また、このリポジトリは **Public** です。ファイル名やパスによる例外はなく、**すべてのファイルが組織外の誰からでも閲覧できます**。

## 配置するファイルの方針

このリポジトリでは、以下のファイルを管理します。

| パス | 役割 | 影響範囲 |
| --- | --- | --- |
| `profile/README.md` | Organization トップページに表示されるプロフィール | 全世界に公開 |
| `.github/ISSUE_TEMPLATE/` | issue テンプレート | 配下リポジトリに継承 |
| `.github/PULL_REQUEST_TEMPLATE.md` | Pull Request テンプレート | 配下リポジトリに継承 |
| `SECURITY.md` | セキュリティポリシー・脆弱性の報告先 | 配下リポジトリに継承 |
| `CODE_OF_CONDUCT.md` | 行動規範 | 配下リポジトリに継承 |
| `workflow-templates/` | GitHub Actions のワークフローテンプレート | 新規ワークフロー作成時の選択肢として表示 |

### スコープ外

- 個別プロダクト固有の開発規約・設定 → 各リポジトリで管理する
- 社内の業務ドキュメントやナレッジ → このリポジトリは Public のため置かない

## 継承の仕組み

### 探索される場所

GitHub は各ファイルを次の順で探索します。どこに置いても機能します。

1. `.github/` ディレクトリ
2. リポジトリのルート
3. `docs/` ディレクトリ

ただし **issue テンプレートだけは `.github/ISSUE_TEMPLATE/` 固定** です。

> [!NOTE]
> リポジトリ名の `.github` と、リポジトリ内の `.github/` ディレクトリは別物です。名前が同じで紛らわしいため、パスを扱うときは注意してください。

### 上書きのルール

継承されるのは、**配下のリポジトリに同名のファイルが存在しない場合のみ** です。各リポジトリが独自のファイルを持っていれば、そちらが優先されます。

つまりこのリポジトリのテンプレートを変更しても、独自テンプレートを持つリポジトリには反映されません。「変更したのに反映されない」ときは、まず対象リポジトリに同名ファイルがないか確認してください。

### ラベルは継承されない

継承されるのは **ファイル** だけです。ラベルはリポジトリごとに管理されており、このリポジトリから配布することはできません。

issue テンプレートには起票時に自動付与するラベルを指定していますが、**指定したラベルが対象リポジトリに存在しない場合、そのラベルは付与されません**（GitHub はラベルを自動作成しません）。

| テンプレート | 指定ラベル | 備考 |
| --- | --- | --- |
| 機能要望 | `enhancement` | GitHub がリポジトリ作成時に自動生成するラベル |
| バグ報告 | `bug` | 同上 |
| 質問 | `question` | 同上 |
| 開発タスク | `chore` | **デフォルトには存在しないため、リポジトリごとに作成が必要** |

`chore` ラベルが必要なリポジトリでは、次のいずれかの方法で作成してください。設定する値はどちらの方法でも同じです。

| 項目 | 設定する値 |
| --- | --- |
| Label name | `chore` |
| Description | `Refactoring, dependency updates, and other maintenance` |
| Color | `#ededed` |

#### ブラウザで作成する場合

1. 対象リポジトリの **Issues** タブを開く
2. 検索ボックス右の **Labels** ボタンを押す（URL を直接開く場合は `https://github.com/gizumo-inc/<リポジトリ名>/labels`）
3. 右上の **New label** ボタンを押す
4. 上表の値を入力する
   - Color 欄は初期状態でランダムな色が入っています。欄をクリックして `#ededed` を直接入力してください
   - 色の指定は必須ではありません。組織内で見分けやすさを揃えたいため、上記の値を推奨しています
5. **Create label** ボタンを押す

#### `gh` コマンドで作成する場合

```sh
gh label create chore --description "Refactoring, dependency updates, and other maintenance" --color ededed
```

> [!NOTE]
> `gh` コマンドでは Color に `#` を付けません。ブラウザの入力欄では `#` 付きで入力します。

### Private にはできない

GitHub の仕様上、[Private な `.github` リポジトリはサポートされていません](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)。Private にすると継承そのものが機能しなくなります。

**「テンプレートを全社に配る」ことと「中身を非公開にする」ことは両立しません。** Public であることを前提に運用してください。

組織メンバーだけに見せたい内容がある場合は、別途 `.github-private` リポジトリ（Private）を作成し、その `profile/README.md` に記載します。この内容は Organization トップページの「Member ビュー」に表示され、メンバーにのみ見えます。ただし用途はプロフィールに限られ、上表のような設定ファイルの継承元にはなりません。

## 変更の手順

1. `main` から作業ブランチを作成する
2. 変更をコミットする
3. Pull Request を作成する
4. レビュー後に `main` へマージする

`main` への直接 push は行いません。変更が全社に波及する性質上、必ず他者の目を通してください。

マージ後は **即座に全リポジトリへ反映されます**。デプロイなどの反映作業はありません。

## 動作確認の方法

テンプレート類はこのリポジトリ内では動作を確認できません。マージ後、配下の任意のリポジトリで実際に画面を開いて確認してください。

| 対象 | 確認方法 |
| --- | --- |
| issue テンプレート | 任意のリポジトリで issue の新規作成画面を開く |
| PR テンプレート | 任意のリポジトリでブランチを push し、PR 作成画面を開く |
| `profile/README.md` | Organization のトップページを開く |
| ポリシー系ファイル | リポジトリの Insights → Community Standards を開く |

## 注意事項

### 機密情報を書かない

Public リポジトリのため、以下は記載できません。

- 社内システム名・内部ツール名
- 個人名、メールアドレスなどの個人情報
- 認証情報、APIキー、トークン
- 取引先名、契約に関わる情報

記載が必要な内容は非公開の場所で管理し、このリポジトリからリンクもしないでください。

### 既存テンプレートの破壊的変更を避ける

テンプレートの項目削除や大幅な構成変更は、変更後に作成される issue / PR すべてに影響します。運用中のリポジトリへの影響を確認したうえで行ってください。

## 参考

- [Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)
- [Customizing your organization's profile](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/customizing-your-organizations-profile)
- [Creating starter workflows for your organization](https://docs.github.com/en/actions/using-workflows/creating-starter-workflows-for-your-organization)

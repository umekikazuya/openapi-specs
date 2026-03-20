# openapi-specs

OpenAPI 3.1.0 共通コンポーネントリポジトリ。各プロジェクトの `openapi.yaml` から Raw URL で `$ref` 参照して使う。

> **バージョニング**: URL 内のタグ (`v1`, `v2`, ...) を固定することで、破壊的変更から各プロジェクトを保護する。
> アップグレードは各プロジェクト側で URL のタグ番号を変更する。詳細は[バージョニングポリシー](#バージョニングポリシー)を参照。

## Quick Start

よく使うコンポーネントのみ参照する最小構成:

```yaml
components:
  schemas:
    ProblemDetails:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/schemas/ProblemDetails.yaml"
  responses:
    BadRequest:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/responses/BadRequest.yaml"
    NotFound:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/responses/NotFound.yaml"
    InternalServerError:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/responses/InternalServerError.yaml"
```

全コンポーネントを参照する場合は[コンポーネント一覧](#コンポーネント一覧)の表を参照。

## コンポーネント一覧

### schemas

| ファイル                  | 説明                                                                         | いつ使うか                                         |
| ------------------------- | ---------------------------------------------------------------------------- | -------------------------------------------------- |
| `ProblemDetails.yaml`     | RFC 9457 準拠のエラーオブジェクト                                            | 全エラーレスポンスのボディ型として使う             |
| `InvalidParam.yaml`       | バリデーションエラー詳細 (`ProblemDetails.invalidParams` の要素)             | 400 系でフィールド単位のエラー詳細を返すときに使う |
| `CursorPageMetadata.yaml` | カーソルベースページネーションのメタデータ (`nextPageToken`, `totalResults`) | 一覧取得エンドポイントのレスポンスに使う           |

```yaml
components:
  schemas:
    ProblemDetails:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/schemas/ProblemDetails.yaml"
    InvalidParam:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/schemas/InvalidParam.yaml"
    CursorPageMetadata:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/schemas/CursorPageMetadata.yaml"
```

### responses

全レスポンスに `X-Request-ID` レスポンスヘッダと `application/problem+json` を含む。

| ファイル                   | ステータス | 説明                 | いつ使うか                           |
| -------------------------- | ---------- | -------------------- | ------------------------------------ |
| `BadRequest.yaml`          | 400        | リクエスト検証エラー | 入力バリデーション失敗時             |
| `Unauthorized.yaml`        | 401        | 認証エラー           | 未認証・トークン期限切れ時           |
| `Forbidden.yaml`           | 403        | 権限不足             | 認証済みだがリソースへのアクセス不可 |
| `NotFound.yaml`            | 404        | リソース不在         | ID 指定のリソースが存在しないとき    |
| `Conflict.yaml`            | 409        | 競合                 | 重複登録・楽観ロック失敗時           |
| `InternalServerError.yaml` | 500        | サーバーエラー       | 予期しないエラー全般                 |

```yaml
components:
  responses:
    BadRequest:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/responses/BadRequest.yaml"
    Unauthorized:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/responses/Unauthorized.yaml"
    Forbidden:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/responses/Forbidden.yaml"
    NotFound:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/responses/NotFound.yaml"
    Conflict:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/responses/Conflict.yaml"
    InternalServerError:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/responses/InternalServerError.yaml"
```

### parameters

| ファイル              | 場所   | 説明                                                     | いつ使うか                               |
| --------------------- | ------ | -------------------------------------------------------- | ---------------------------------------- |
| `PathId.yaml`         | path   | `/{id}` UUID v4 パスパラメータ                           | 単一リソース取得・更新・削除のパスに使う |
| `PageToken.yaml`      | query  | `?pageToken=` カーソルトークン                           | 一覧取得のページネーションに使う         |
| `MaxResults.yaml`     | query  | `?maxResults=` 件数上限 (default: 20, max: 100)          | 一覧取得の件数制御に使う                 |
| `XRequestedWith.yaml` | header | CSRF 対策 (`X-Requested-With: XMLHttpRequest`、required) | 認証が必要な全ミューテーション操作に使う |
| `XRequestId.yaml`     | header | ログ追跡用 (`X-Request-ID`、optional)                    | 分散トレーシングが必要なエンドポイントに |

```yaml
components:
  parameters:
    PathId:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/parameters/PathId.yaml"
    PageToken:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/parameters/PageToken.yaml"
    MaxResults:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/parameters/MaxResults.yaml"
    XRequestedWith:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/parameters/XRequestedWith.yaml"
    XRequestId:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/parameters/XRequestId.yaml"
```

### headers

| ファイル          | 説明                                | いつ使うか                               |
| ----------------- | ----------------------------------- | ---------------------------------------- |
| `XRequestId.yaml` | `X-Request-ID` レスポンスヘッダ定義 | レスポンスにリクエスト ID を含めるときに |

```yaml
components:
  headers:
    XRequestId:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/v1/_common/headers/XRequestId.yaml"
```

## 設計ガイドライン

### Google JSON Style Guide 準拠

- **命名規則**: プロパティ名はすべて `camelCase`
- **日時形式**: ISO 8601 (`YYYY-MM-DDTHH:mm:ssZ`) かつ UTC
- **ID形式**: UUID v4
- **空の値**: `null` は使わず、プロパティ自体を省略

### 認証 (HttpOnly Cookies)

| Cookie (プロジェクト固有の名前に変更) | 用途                 | 有効期限                   |
| ------------------------------------- | -------------------- | -------------------------- |
| `accessToken` (例)                    | アクセストークン     | 15分 (一般) / 5分 (管理者) |
| `refreshToken` (例)                   | リフレッシュトークン | 30日                       |

### CSRF 対策

認証が必要な全リクエストに `X-Requested-With: XMLHttpRequest` を付与する。

### ページネーション (カーソルベース・前進のみ)

```
GET /resources?pageToken=<token>&maxResults=20
```

レスポンス例:

```json
{
  "items": [...],
  "metadata": {
    "nextPageToken": "eyJpZCI6...",
    "totalResults": 150
  }
}
```

`nextPageToken` が省略されている場合は最終ページ。前のページへの遡りはサポートしない。

### エラーレスポンス (RFC 9457)

```json
{
  "type": "https://example.com/probs/validation-error",
  "title": "Validation Error",
  "status": 400,
  "detail": "The 'email' field must be a valid email address.",
  "instance": "/users",
  "invalidParams": [
    { "name": "email", "reason": "must be a valid email address" }
  ]
}
```

## バージョニングポリシー

タグ (`v1`, `v2`, ...) は破壊的変更が生じたときにのみ上げる。

**破壊的変更 (タグを上げる)**:

- フィールドの削除
- フィールドの型変更
- `required` への追加

**非破壊的変更 (同タグへの上書き可)**:

- フィールドの追加
- `description` の修正
- `example` の追加・変更

タグの切り方:

```bash
git tag v2
git push origin v2
```

## ローカル開発 (example)

`example/` に動作確認用のサンプル仕様書がある。

```bash
# Spectral で lint
npm install -g @stoplight/spectral-cli
spectral lint example/openapi.yaml

# Prism でモックサーバー起動 (http://localhost:4010)
npm install -g @stoplight/prism-cli
prism mock example/openapi.yaml
```
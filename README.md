# openapi-specs

OpenAPI 3.1.0 共通コンポーネントリポジトリ。
各プロジェクトの `openapi.yaml` から Raw URL で `$ref` 参照して使う。

## 使い方

各プロジェクトの `openapi.yaml` に以下のように記述する。

```yaml
components:
  schemas:
    ProblemDetails:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/schemas/ProblemDetails.yaml"
    ListResponseMetadata:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/schemas/ListResponseMetadata.yaml"
  responses:
    BadRequest:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/responses/BadRequest.yaml"
    Unauthorized:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/responses/Unauthorized.yaml"
    Forbidden:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/responses/Forbidden.yaml"
    NotFound:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/responses/NotFound.yaml"
    Conflict:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/responses/Conflict.yaml"
    InternalServerError:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/responses/InternalServerError.yaml"
  parameters:
    PathId:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/parameters/PathId.yaml"
    PageToken:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/parameters/PageToken.yaml"
    MaxResults:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/parameters/MaxResults.yaml"
    XRequestedWith:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/parameters/XRequestedWith.yaml"
    XRequestId:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/parameters/XRequestId.yaml"
  headers:
    XRequestId:
      $ref: "https://raw.githubusercontent.com/umekikazuya/openapi-specs/main/_common/headers/XRequestId.yaml"
```

## コンポーネント一覧

### schemas

| ファイル                    | 説明                                                                         |
| --------------------------- | ---------------------------------------------------------------------------- |
| `ProblemDetails.yaml`       | RFC 9457 準拠のエラーオブジェクト                                            |
| `InvalidParam.yaml`         | バリデーションエラー詳細 (`ProblemDetails.invalidParams` の要素)             |
| `ListResponseMetadata.yaml` | カーソルベースページネーションのメタデータ (`nextPageToken`, `totalResults`) |

### responses

| ファイル                   | ステータス | 説明                 |
| -------------------------- | ---------- | -------------------- |
| `BadRequest.yaml`          | 400        | リクエスト検証エラー |
| `Unauthorized.yaml`        | 401        | 認証エラー           |
| `Forbidden.yaml`           | 403        | 権限不足             |
| `NotFound.yaml`            | 404        | リソース不在         |
| `Conflict.yaml`            | 409        | 競合                 |
| `InternalServerError.yaml` | 500        | サーバーエラー       |

全レスポンスに `X-Request-ID` レスポンスヘッダと `application/problem+json` を含む。

### parameters

| ファイル              | 場所   | 説明                                                     |
| --------------------- | ------ | -------------------------------------------------------- |
| `PathId.yaml`         | path   | `/{id}` UUID v4 パスパラメータ                           |
| `PageToken.yaml`      | query  | `?pageToken=` カーソルトークン                           |
| `MaxResults.yaml`     | query  | `?maxResults=` 件数上限 (default: 20, max: 100)          |
| `XRequestedWith.yaml` | header | CSRF 対策 (`X-Requested-With: XMLHttpRequest`、required) |
| `XRequestId.yaml`     | header | ログ追跡用 (`X-Request-ID`、optional)                    |

### headers

| ファイル          | 説明                                |
| ----------------- | ----------------------------------- |
| `XRequestId.yaml` | `X-Request-ID` レスポンスヘッダ定義 |

## 設計ガイドライン

### Google JSON Style Guide 準拠

- **命名規則**: プロパティ名はすべて `camelCase`
- **日時形式**: ISO 8601 (`YYYY-MM-DDTHH:mm:ssZ`) かつ UTC
- **ID形式**: UUID v4
- **空の値**: `null` は使わず、プロパティ自体を省略

### 認証 (HttpOnly Cookies)

| Cookie               | 用途                 | 有効期限                   |
| -------------------- | -------------------- | -------------------------- |
| `tayoriAccessToken`  | アクセストークン     | 15分 (一般) / 5分 (管理者) |
| `tayoriRefreshToken` | リフレッシュトークン | 30日                       |

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

## ローカル開発 (example)

`example/` に動作確認用のサンプル仕様書がある。

```bash
pnpm install

# Spectral で lint
pnpm run lint

# Prism でモックサーバー起動 (http://localhost:4010)
pnpm run mock
```

## ファイル構成

```
_common/
  schemas/
    ProblemDetails.yaml
    InvalidParam.yaml
    ListResponseMetadata.yaml
  responses/
    BadRequest.yaml
    Unauthorized.yaml
    Forbidden.yaml
    NotFound.yaml
    Conflict.yaml
    InternalServerError.yaml
  parameters/
    PathId.yaml
    PageToken.yaml
    MaxResults.yaml
    XRequestedWith.yaml
    XRequestId.yaml
  headers/
    XRequestId.yaml
example/
  openapi.yaml          # 動作確認用サンプル
  paths/
    health.yaml
    items.yaml
.spectral.yaml
package.json
```
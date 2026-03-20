# Changelog

フォーマットは [Keep a Changelog](https://keepachangelog.com/ja/1.0.0/) に基づく。

## v1 (2026-03-20)

### Added

- `ProblemDetails.yaml` — RFC 9457 準拠のエラーオブジェクト
- `InvalidParam.yaml` — バリデーションエラー詳細 (`ProblemDetails.invalidParams` の要素)
- `CursorPageMetadata.yaml` — カーソルベースページネーションのメタデータ (`nextPageToken`, `totalResults`)
- `BadRequest.yaml` (400) — リクエスト検証エラーレスポンス
- `Unauthorized.yaml` (401) — 認証エラーレスポンス
- `Forbidden.yaml` (403) — 権限不足レスポンス
- `NotFound.yaml` (404) — リソース不在レスポンス
- `Conflict.yaml` (409) — 競合レスポンス
- `InternalServerError.yaml` (500) — サーバーエラーレスポンス
- `PathId.yaml` — UUID v4 パスパラメータ (`/{id}`)
- `PageToken.yaml` — カーソルトークンクエリパラメータ (`?pageToken=`)
- `MaxResults.yaml` — 件数上限クエリパラメータ (`?maxResults=`, default: 20, max: 100)
- `XRequestedWith.yaml` — CSRF 対策リクエストヘッダ
- `XRequestId.yaml` (parameter) — ログ追跡用リクエストヘッダ
- `XRequestId.yaml` (header) — `X-Request-ID` レスポンスヘッダ定義

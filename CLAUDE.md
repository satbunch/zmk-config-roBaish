# CLAUDE.md — zmk-config-roBaish 運用ガイド

このリポジトリは `kumamuk-git/zmk-config-roBa` からフォークした ZMK ファームウェア設定。
ユーザーの自作キーボードキット「roBaish」もこの構成を使用する。

## キーマップ編集の役割分担

`config/roBa.keymap` は以下の構成:

```
#include群
&mt { ... }         // hold-tapのフレーバー設定
&trackball { ... }  // トラックボール設定
/ {
    combos { ... }     // コンボ定義
    macros { ... }     // マクロ定義
    behaviors { ... }  // カスタムビヘイビア定義
    keymap { ... }     // 各レイヤーのbindings
};
```

- **`keymap { }` ノード配下（各レイヤーの `bindings`）** → KeymapEditor（Web GUI）で編集する。ユーザーが確認しながら設定するため、Claudeはここを直接編集しない。
- **それ以外**（`combos`, `macros`, `behaviors`, `&trackball` のプロパティ、`boards/shields/roBa/` 配下のハードウェア定義など）→ KeymapEditorの対象外なので、リポジトリを直接編集してよい。

作業時は、変更が `keymap{}` ノード内かどうかを必ず確認し、ノード内の変更が必要な場合はユーザーに確認する。

## ZMK Studio との関係

`roBa_R.conf` で `CONFIG_ZMK_STUDIO=y`（ロック無効）が有効。ZMK Studioは実機にUSB接続してライブ編集するツールで、変更は実機の設定パーティションに保存され、**リポジトリの`.keymap`には自動反映されない**。

- KeymapEditor（リポジトリ側）を正とする。
- ZMK Studioで実機を直接編集した場合、気に入った変更は都度KeymapEditor側（リポジトリ）に手動で反映すること。反映を忘れると、次回ファームウェア書き込み時にStudioでの変更が失われる。

## ブランチ運用

- **小規模な変更**（設定値の調整、バグ修正など）: `main` に直接コミットしてよい。
- **大規模な変更**（レイヤー構成の変更など実機での動作検証が必要なもの）: 作業ブランチを切り、実機で動作確認してから `main` にマージする。

## upstream同期

- `upstream` remote: `kumamuk-git/zmk-config-roBa`（追加済み、fetch専用）
- **追従する**: `boards/shields/roBa/` 配下のハードウェア定義、`config/west.yml` などビルド関連の修正。
- **追従しない**: `config/roBa.keymap`。ユーザー独自のキーマップとして運用するため、upstreamの変更とはマージしない。

## メンテナンス

このファイルが長くなった場合は、トピックごとに `docs/` 配下のファイルに分割し、ここには目次と要点だけを残す。

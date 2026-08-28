# CLAUDE.md（samepage）

## ブランチ運用
- 更新は `develop` ブランチで行う。作業開始時に `git branch --show-current` を確認し、`develop` でなければ切り替える。
- `main` へのマージ・直接コミット・push は AI は行わない（メンテナが行う）。PR 作成やマージは提案までに留める。

## 命名・契約
- `data-sp-*` / `sp-` プレフィックス、JSON フィールド名は `docs/design.md` と `SKILL.md` が正。README 側で勝手に変えない。

## README
- `README.md` と `README.ja.md` は節単位で同期させる。片方だけ変更しない。
- `docs/images/demo.gif` は `python3 docs/build_demo_gif.py` で再生成する（手で編集しない）。

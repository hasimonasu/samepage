# CLAUDE.md（samepage）

## 設計上の SSOT
- 設計判断に迷ったら、まず `docs/alignment/INDEX.md` を見る。確定している決定を勝手に覆さない。
- 覆す必要があるときは、根拠となる新しい事実を添えて該当の ALIGNMENT 文書で「取り消して再検討」する（手順は `skills/grill-on-samepage/SKILL.md` §4）。黙って別の判断で進めない。
- `docs/alignment/INDEX.md` は生成物。手で編集せず、`python3 docs/build_alignment_html.py --index docs/alignment` で作り直す。

## ブランチ運用
- 更新は `develop` ブランチで行う。作業開始時に `git branch --show-current` を確認し、`develop` でなければ切り替える。
- `feature/*` から `develop` へのマージは AI が行ってよい。
- **AI は push を行わない。** `main` へのマージ・直接コミットも行わない（メンテナが行う）。`main` に関する PR 作成やマージは提案までに留める。

## 命名・契約
- `data-sp-*` / `sp-` プレフィックス、JSON フィールド名は `docs/design.md` と `skills/samepage/SKILL.md` が正。README 側で勝手に変えない。

## スキル構成
- リポジトリは `.claude-plugin/plugin.json` を持つプラグイン。スキルは `skills/<name>/SKILL.md` に置く。ルートに `SKILL.md` を置いても読み込まれない。
- スキル内から同梱スクリプトを参照するときは `${CLAUDE_PLUGIN_ROOT}` を使う（`${CLAUDE_SKILL_DIR}` はスキル自身のディレクトリを指すので届かない）。

## README と生成物
- `README.md` と `README.ja.md` は節単位で同期させる。片方だけ変更しない。
- `README.html` / `README.ja.html` / `docs/alignment/*.html` / `docs/alignment/INDEX.md` は生成物。元の `.md` を直して再生成する。**CI の `docs` ジョブが鮮度を検査するので、再生成を忘れると落ちる。** 手順は README の「開発 / Development」節にある。
- `docs/images/demo.gif` は `python3 docs/build_demo_gif.py` で再生成する（手で編集しない）。

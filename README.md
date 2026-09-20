# my-skills

Codex、Claude Codeなど、Agent Skills形式に対応したエージェントで再利用するためのSkill集です。

## 構成

```text
my-skills/
└── skills/
    └── <skill-name>/
        ├── SKILL.md          # 必須
        ├── scripts/          # 任意: 実行用スクリプト
        ├── references/       # 任意: 補足資料
        ├── assets/           # 任意: テンプレートなど
        └── agents/
            └── openai.yaml   # 任意: Codex向けメタデータ
```

必要のないディレクトリは作成しません。

## Skillの追加

`skills/<skill-name>/SKILL.md` を作成します。名前には小文字、数字、ハイフンを使用します。

```markdown
---
name: example-skill
description: このSkillが何を行い、どのような依頼で使用するかを簡潔に記述します。
---

# Instructions

エージェントが従う手順や判断基準を記述します。
```

`name` と `description` は必須です。補足資料やスクリプトは、実際に必要になった場合だけ追加します。

## 別プロジェクトへの導入

公開後は、`skills` CLIを使ってCodexとClaude Codeの両方へ導入できます。

```bash
npx skills add <owner>/my-skills \
  --agent codex \
  --agent claude-code
```

特定のSkillだけを導入する場合:

```bash
npx skills add <owner>/my-skills \
  --skill <skill-name> \
  --agent codex \
  --agent claude-code
```

手動で配置する場合、プロジェクト内の配置先は次のとおりです。

- Codex: `.agents/skills/<skill-name>/`
- Claude Code: `.claude/skills/<skill-name>/`

## 運用方針

- 共通の手順は標準的な`SKILL.md`に記述する
- エージェント固有の設定は共通手順から分離する
- 外部Skillを追加する前に、`SKILL.md`と同梱スクリプトを確認する
- 変更後は対象Skillを実際の依頼で動作確認する

## ライセンス

ライセンスは未設定です。公開または共有する前に、用途に合うライセンスを追加してください。


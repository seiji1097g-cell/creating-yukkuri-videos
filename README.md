# creating-yukkuri-videos

オリジナルキャラクター「こむぎ」と「あずき」が掛け合いで解説する「ゆっくり解説風」の動画（MP4）を、Claude に作ってもらうための [Agent Skill](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) です。

「〇〇でゆっくり解説動画を作って」と頼むだけで、Claude が次のところまで進めます。

- テーマの調査（Web検索で事実と出典を集める）
- 構成と台本づくり（台本は動画にする前に確認できる）
- キャラクターの描画（表情5種、口パク、まばたき）
- 音声合成（Open JTalk）、字幕、解説パネル、図
- MP4・字幕（SRT）・サムネイル（1280×720）・YouTube 概要欄の書き出し

キャラクターはコードで描くオリジナルなので、立ち絵素材を用意する必要はありません。

## キャラクター

| | こむぎ（聞き役・画面右） | あずき（解説役・画面左） |
|---|---|---|
| 見た目 | クリーム色のまんじゅう型、頭に双葉、赤いほっぺ | 藤色のまんじゅう型、丸メガネ、太めの眉 |
| 性格 | 好奇心が強く、視聴者が思いそうな疑問を口にする | 落ち着いた物知り。たとえ話で説明する |
| 口調 | 一人称「わたし」。「〜なの？」「へぇ〜！」 | 一人称「ぼく」。「〜なんだ」「いい質問だね」 |

自分で用意した立ち絵画像を添付して「これを使って」と頼めば、その画像で作ることもできます。

## 必要なもの

- コードを実行できる Claude（claude.ai のコード実行・ファイル作成を有効にした状態、または Claude Code）
- 動画を作る環境に Python 3、ffmpeg、日本語フォント（Noto Sans CJK JP）
- Python パッケージ：Pillow、numpy、pyopenjtalk（足りなければ Claude が入れます）

## インストール

### claude.ai

1. [Releases](../../releases) から `creating-yukkuri-videos.zip` をダウンロードします。
   - 緑の「Code」ボタンの「Download ZIP」は、フォルダ名が `…-main` になるためアップロードできません。
2. claude.ai で **Customize → Skills** を開き、「+」→「Create skill」→「Upload a skill」から ZIP をアップロードします。

### Claude Code

スキルのフォルダに clone します（このリポジトリの URL は「Code」ボタンからコピーできます）。

```bash
git clone <このリポジトリのURL> ~/.claude/skills/creating-yukkuri-videos
```

## 使い方

Claude にこう頼みます。

- 「ブラックホールでゆっくり解説動画を作って」
- 「こむぎとあずきで、円安のしくみを3分で解説して」
- 「〇〇を解説する動画を一気に作って」（台本の確認を省略）

指定がなければ、長さは約3分、台本ができた時点で一度確認を求めます。

## しくみ

このスキルは `SKILL.md` 1ファイルだけでできています。動画を作るコード（キャラクターを描く `chars.py` と、音声・字幕・画面を作って書き出す `render.py`）は `SKILL.md` の末尾に入っていて、Claude が作業フォルダに書き出して実行します。

## 姉妹スキル

霊夢と魔理沙（たぬき式ゆっくりの立ち絵）で作る版もあります。

- [yukkuri-reimu-marisa-videos](https://github.com/seiji1097g-cell/yukkuri-reimu-marisa-videos)

## ライセンス

- `SKILL.md`（中のコードを含む）とこの README：[MIT License](LICENSE)
- 音声は [Open JTalk](https://open-jtalk.sourceforge.net/) と HTS Voice "Mei"（© Nagoya Institute of Technology、CC BY 3.0）で合成します。音声モデルはこのリポジトリには含まれず、pyopenjtalk が取得します。作った動画を公開するときは、概要欄などに次のクレジットを入れてください（スキルが `description.txt` に入れて出力します）。
  - 音声合成：Open JTalk ／ 音声モデル：HTS Voice "Mei"（© Nagoya Institute of Technology, CC BY 3.0）

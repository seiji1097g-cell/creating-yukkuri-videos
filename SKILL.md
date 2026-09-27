---
name: "creating-yukkuri-videos"
description: "指定テーマを調べ、オリジナルキャラ「こむぎ」と「あずき」の2人が掛け合いで解説する「ゆっくり解説風」の動画（MP4）を台本・音声・字幕・サムネイルまで作る。「〇〇でゆっくり解説動画を作って」「〇〇を解説する動画を作って」などで使う。"
---

# ゆっくり解説風の動画を作る

ユーザーが指定したテーマについて、2人のキャラクターが掛け合いで解説する「ゆっくり解説風」の動画を作る。調査、台本、キャラクター、音声、MP4の書き出しまで、すべてコードを実行できるこの環境で行う。下の「コード」の chars.py と render.py は動作確認済みなので、そのまま使う。

## 入力
- テーマ（必須）：わからなければユーザーに聞く。
- 動画の長さ：指定がなければ3分（±20%まで可）。
- 想定視聴者：指定がなければ、前提知識のない中学生〜大人。
- 台本の確認：指定がなければ「あり」（台本ができたら見せて、OKをもらってから動画にする）。「一気に作って」などと言われたら「なし」。
- キャラクター画像：立ち絵画像が添付されていれば「立ち絵画像を使う場合」に従う。

## 守ること
- AquesTalk、SofTalk、棒読みちゃん、ゆっくりMovieMakerなど、ゆっくり動画用のツールは使わない。
- 事実はWeb検索で確かめ、確かめられなかったことは台本に入れない。数字、年、人名、固有名詞には出典を残す。
- ネット上の画像、音楽、効果音は使わない。図やイラストはコードで描く。
- キャラクターは、既存の作品のキャラクター（東方Projectのキャラクター、ゆっくり霊夢・ゆっくり魔理沙を含む）に似せない。標準のキャラクターは chars.py のオリジナルの2人を使う。
- 途中で問題が起きたら別の方法で解決して先へ進み、最後にまとめて報告する。

## 手順1：調査
1. テーマについてWeb検索し、信頼できる情報源（公的機関、研究機関、大学、百科事典、大手メディアなど）を優先して事実を集める。
2. 視聴者が「へぇ」と思うポイントを3〜5個選び、それぞれの根拠と出典URLをメモする。
3. 説が分かれていることや、はっきりしていないことは、台本でもそのとおりに言う。

## 手順2：構成
- 冒頭（約15秒）：あいさつと、この動画を見ると何がわかるかの予告。視聴者の興味を引く疑問から入る。
- 本編：章を3〜5個に分ける。1つの章で伝える要点は1つに絞る。
- まとめ（約20秒）：要点を3行でふり返る。
- 締め（約5秒）：締めのあいさつ。

## 手順3：台本
| | こむぎ（聞き役・画面右） | あずき（解説役・画面左） |
|---|---|---|
| 見た目 | クリーム色のまんじゅう型、頭に双葉、赤いほっぺ | 藤色のまんじゅう型、丸メガネ、太めの眉 |
| 性格 | 好奇心が強く、視聴者が思いそうな疑問をそのまま口にする。ときどきツッコミを入れる | 落ち着いた物知り。たとえ話でわかりやすく説明する |
| 口調 | 一人称「わたし」。「〜なの？」「へぇ〜！」「ちょっと待って」 | 一人称「ぼく」。「〜なんだ」「〜だよ」「いい質問だね」 |
| 声 | 高めで少し速い | 低めで落ち着いた声 |

台本のルール
- 1つのセリフは全角35字以内（多くても40字）。長くなるときは分ける。
- 「こむぎの質問 → あずきの説明 → こむぎの反応」を基本の流れにし、3〜4セリフごとに、たとえ話、驚き、ツッコミ、クイズなどで変化をつける。
- 専門用語は、出てきたらすぐに説明する。
- 長さの目安は、1分あたり約350字。

台本は作業フォルダに script.json として、次の形で保存する。
```json
{
  "title": "動画タイトル（30字以内）",
  "thumbnail_text": "サムネイルの文字（省略するとタイトル）",
  "colors": {"bg_top": [120, 185, 240], "bg_bottom": [215, 238, 252], "accent": [40, 70, 130], "line": [90, 140, 200]},
  "chapters": [{"id": 1, "title": "章の見出し"}],
  "lines": [
    {
      "speaker": "komugi",
      "text": "字幕に出す文",
      "yomi": "読み上げ用の文（読み間違えそうな語をひらがなに直したもの。直す必要がなければ text と同じ）",
      "face": "normal",
      "chapter": 1,
      "panel": {"type": "bullets", "heading": "見出し", "items": ["項目1", "項目2"]}
    }
  ],
  "sources": [{"title": "出典名", "url": "https://..."}]
}
```
- colors：テーマに合う背景の色（上・下）、見出しの色（accent）、線の色（line）。省略すると空色。
- speaker：komugi か azuki。
- face：normal（ふつう）、smile（笑顔）、surprise（驚き）、trouble（困り）、think（考え中）。
- chapter：chapters の id。
- panel：画面中央の解説パネルを切り替えるセリフにだけ書く。書かないセリフでは前のパネルをそのまま出す。最初のセリフには {"type": "title"} を付ける。
  - title：動画タイトル
  - heading：章の見出し（{"type": "heading", "heading": "…"}）。章の最初のセリフに付ける
  - bullets：見出しと箇条書き（4項目まで、1項目20字まで）
  - keyword：大きく見せる一語や数字と補足（{"type": "keyword", "keyword": "約1億5000万km", "sub": "地球から太陽まで"}）
  - diagram：図（{"type": "diagram", "name": "関数名", "heading": "図の見出し", "spec": "何を描くか"}）。手順4で diagrams.py に描く関数を書く

「台本の確認：あり」のときは、ここで台本を「話し手／セリフ／パネル」の表にしてユーザーに見せ、OKをもらってから次へ進む。

## 手順4：制作ファイルを用意する
1. 作業フォルダに、下の「コード」の chars.py と render.py を一字一句そのまま保存する。
2. `pip install pyopenjtalk`（環境によっては `--break-system-packages` を付ける）。初めて使うときに辞書を自動でダウンロードする。ffmpeg と日本語フォント（Noto Sans CJK JP）があるか確かめ、なければ入れる。
3. diagram パネルがあれば、script.json と同じフォルダに diagrams.py を作る。
```python
# diagrams.py の例
def scatter_bar(d, box, font):
    """d：ImageDraw、box：(x0, y0, x1, y1) 描いてよい範囲、font(size, black=False)：日本語フォント"""
    x0, y0, x1, y1 = box
    ...  # Pillow の図形と文字で描く。数値ラベルが見出しや枠からはみ出さないよう、box の中に収める

DIAGRAMS = {"scatter_bar": scatter_bar}
```

## 手順5：確認モードで直す
`python3 render.py script.json 出力フォルダ --check` を実行する（動画は作らず、数十秒で終わる）。出力を見て直し、問題がなくなるまでくり返す。
1. 読み：[読みの確認] のカタカナを1行ずつ見て、読み間違いは yomi のその語をひらがなに直す。
   - 例：「太陽の光は」→「コーハ」と読まれた → 「太陽のひかりは」。「その間に」→「ソノカンニ」と読まれた → 「そのあいだに」。
   - 英字、数字、固有名詞、同じ字で読みが複数ある漢字（間、光、方、生 など）は特に注意する。英単語はカタカナで書く。
2. 長さ：[長さ] が目標から±20%より外れたら、台本を足したり削ったりする。
3. 字幕：[字幕の改行] で、48px 以下になったセリフや不自然な改行があれば、セリフを短くするか2つに分ける。
4. [注意] に出たもの（文字が長すぎるパネル、関数のない図など）を直す。
5. work/check.png（主な場面）と work/characters.png（全表情）を画像として開いて自分の目で見る。文字のはみ出しや重なり、図の崩れがあれば直す。

## 手順6：書き出す
`python3 render.py script.json 出力フォルダ` を実行する。動画1分あたり1分弱かかる。コマンドの制限時間を超えそうなとき（目安として5分以上の動画）は、`nohup python3 render.py script.json 出力フォルダ > render.log 2>&1 &` でバックグラウンド実行し、render.log を見て終わるまで待つ。

出力フォルダには video.mp4、subtitles.srt、thumbnail.png ができる。作業ファイルは ./work に置かれる（timeline.json、chapters.txt、check.png など）。

## 手順7：確かめる
- 書き出し後の work/check.png を見て、確認モードのときと変わりないか確かめる。
- `ffprobe` で、映像と音声の両方があり、長さがそろっていることを確かめる。
- 台本の内容が、手順1で集めた出典と食い違っていないか見直す。

## 手順8：渡す
ユーザーが受け取れる出力フォルダに、次のものをそろえて渡す。
1. video.mp4（動画）
2. subtitles.srt（字幕）
3. thumbnail.png（サムネイル、1280×720）
4. script.md（台本：work/timeline.json から「開始時刻／話し手／セリフ」の表にする。話し手は「こむぎ」「あずき」と書く）
5. description.txt（YouTube概要欄用）：内容の紹介、チャプター（work/chapters.txt をそのまま使う）、出典の一覧、クレジット
   - クレジット（必須）：「音声合成：Open JTalk／音声モデル：HTS Voice "Mei"（© Nagoya Institute of Technology, CC BY 3.0）」

最後に、動画の長さ、章立て、途中で起きた問題とその対処を短く報告する。

## 立ち絵画像を使う場合
- 添付された画像をそのまま使い、描き直したり、似た絵を新しく描いたりしない。
- どの画像が聞き役（画面右、speaker は komugi）と解説役（画面左、speaker は azuki）かを決める。わからなければユーザーに聞く。台本のセリフの中では、画像のキャラクターの名前で呼び合う。
- script.json に次を加える（パスは script.json からの相対パス）。差分画像がなければ base だけでよい。話しているキャラクターは上下に弾むので、口パクの差分がなくても話し手はわかる。
```json
"character_images": {
  "komugi": {"base": "img/a.png", "mouth_half": "img/a_half.png", "mouth_open": "img/a_open.png",
             "eyes_closed": "img/a_blink.png", "faces": {"smile": "img/a_smile.png"}},
  "azuki": {"base": "img/b.png"}
},
"extra_credits": ["立ち絵：配布元が求めるクレジット表記"]
```
- 画像の配布元の規約とクレジット表記をユーザーに確認し、extra_credits（エンドカードに出る）と description.txt に入れる。

## コード

### chars.py
```python
"""キャラクターの画像を用意する。
- オリジナルキャラ「こむぎ」「あずき」をPillowで描く（2倍で描いて縮小）
- 立ち絵画像が指定されていれば、その画像を読み込んで使う（描き直さない）
"""
import math, os, subprocess
from PIL import Image, ImageDraw, ImageFont

W = H = 320      # 最終サイズ
S = 2            # 描画倍率
LINE = (74, 48, 32, 255)
FACES = ["normal", "smile", "surprise", "trouble", "think"]
MOUTHS = ["closed", "half", "open"]
EYES = ["open", "closed"]
CHAR = {
    "komugi": {"body": (255, 241, 201, 255), "eye_r": 34, "eye_ry": 40, "eye_dx": 105},
    "azuki":  {"body": (220, 208, 244, 255), "eye_r": 26, "eye_ry": 30, "eye_dx": 100},
}


def find_font(weight="Bold"):
    """Noto Sans CJK JP を探す。.ttc は index=0 が日本語"""
    for d in ("/usr/share/fonts/opentype/noto", "/usr/share/fonts/noto-cjk", "/usr/share/fonts/google-noto-cjk",
              "/usr/share/fonts/truetype/noto", os.path.expanduser("~/.fonts"), os.path.expanduser("~/.local/share/fonts")):
        for name in (f"NotoSansCJK-{weight}.ttc", f"NotoSansCJKjp-{weight}.otf", f"NotoSansJP-{weight}.otf", f"NotoSansJP-{weight}.ttf"):
            if os.path.exists(os.path.join(d, name)):
                return os.path.join(d, name)
    out = subprocess.run(["fc-list", ":lang=ja", "file"], capture_output=True, text=True).stdout.split()
    files = [f.rstrip(":") for f in out if "Sans" in f] or [f.rstrip(":") for f in out]
    if not files:
        raise SystemExit("日本語フォントが見つかりません。fonts-noto-cjk を入れてください（例：apt-get install -y fonts-noto-cjk）")
    return files[0]


def _leaf(d, cx, cy, ang, length, width, fill):
    pts = [(t / 40 * length, math.sin(math.pi * t / 40) * width / 2) for t in range(41)]
    pts += [(x, -y) for x, y in reversed(pts)]
    ca, sa = math.cos(ang), math.sin(ang)
    d.polygon([(cx + x * ca - y * sa, cy + x * sa + y * ca) for x, y in pts], fill=fill, outline=LINE, width=8)


def draw(name, face="normal", mouth="closed", eyes="open"):
    c = CHAR[name]
    im = Image.new("RGBA", (W * S, H * S), (0, 0, 0, 0))
    d = ImageDraw.Draw(im)
    cx = W * S // 2
    d.ellipse((90, 590, 550, 632), fill=(0, 0, 0, 50))                       # 影
    if name == "komugi":                                                     # 双葉
        d.line((cx, 170, cx + 4, 110), fill=LINE, width=10)
        _leaf(d, cx + 2, 115, math.radians(-150), 95, 52, (150, 205, 90, 255))
        _leaf(d, cx + 2, 115, math.radians(-30), 95, 52, (150, 205, 90, 255))
    d.ellipse((40, 160, 600, 610), fill=c["body"], outline=LINE, width=10)  # 体
    d.ellipse((120, 210, 230, 260), fill=(255, 255, 255, 110))              # ハイライト
    ey = 360
    ex = [cx - c["eye_dx"], cx + c["eye_dx"]]
    r, ry = c["eye_r"], c["eye_ry"]
    if face == "surprise":
        r, ry = int(r * 1.25), int(ry * 1.25)
    if name == "komugi":                                                     # ほっぺ
        for x in ex:
            d.ellipse((x - 48, ey + 40, x + 48, ey + 78), fill=(255, 140, 140, 150))
    if name == "azuki" or face in ("trouble", "surprise", "think"):          # 眉
        bw = 14 if name == "azuki" else 9
        for i, x in enumerate(ex):
            s = -1 if i == 0 else 1
            if face == "trouble":      # 八の字（内側が上がる）
                d.line((x + s * 40, ey - 64, x - s * 26, ey - 90), fill=LINE, width=bw)
            elif face == "surprise":
                d.arc((x - 42, ey - 110, x + 42, ey - 60), 200, 340, fill=LINE, width=bw)
            else:
                d.line((x - 38, ey - 70, x + 38, ey - 70 - (8 if face == "think" and i == 1 else 0)), fill=LINE, width=bw)
    for x in ex:                                                             # 目
        if eyes == "closed":
            d.arc((x - r, ey - ry // 2, x + r, ey + ry), 20, 160, fill=LINE, width=9)
        elif face == "smile":
            d.arc((x - r, ey - ry // 2, x + r, ey + ry + 10), 200, 340, fill=LINE, width=11)
        else:
            d.ellipse((x - r, ey - ry, x + r, ey + ry), fill=LINE)
            hx = x - r // 3 + (r // 2 if face == "think" else 0)
            d.ellipse((hx - r // 3, ey - ry // 2 - r // 3, hx + r // 3, ey - ry // 2 + r // 3), fill=(255, 255, 255, 255))
    if name == "azuki":                                                      # 丸メガネ
        for x in ex:
            d.ellipse((x - 62, ey - 58, x + 62, ey + 58), outline=LINE, width=9)
        d.arc((cx - 40, ey - 25, cx + 40, ey + 25), 200, 340, fill=LINE, width=9)
    my = 470                                                                 # 口
    if mouth == "closed":
        if face == "trouble":
            d.line([(cx - 34 + i * 17, my + (6 if i % 2 else -6)) for i in range(5)], fill=LINE, width=8, joint="curve")
        elif face == "surprise":
            d.ellipse((cx - 14, my - 12, cx + 14, my + 16), outline=LINE, width=8)
        else:
            d.arc((cx - 32, my - 30, cx + 32, my + 16), 30, 150, fill=LINE, width=8)
    else:
        hw, hh = (30, 22) if mouth == "half" else (40, 42)
        d.ellipse((cx - hw, my - hh // 2, cx + hw, my + hh), fill=(150, 40, 50, 255), outline=LINE, width=7)
        if mouth == "open":
            d.chord((cx - 26, my + 4, cx + 26, my + hh - 4), 0, 180, fill=(235, 110, 120, 255))
    f = ImageFont.truetype(find_font("Black"), 110, index=0)                 # 記号
    if face == "surprise":
        d.text((545, 110), "！", font=f, fill=(230, 60, 60, 255), stroke_width=6, stroke_fill=(255, 255, 255, 255))
    elif face == "trouble":
        x0, y0 = 540, 230
        d.ellipse((x0 - 26, y0, x0 + 26, y0 + 60), fill=(120, 190, 255, 230), outline=LINE, width=5)
        d.polygon([(x0 - 22, y0 + 20), (x0, y0 - 30), (x0 + 22, y0 + 20)], fill=(120, 190, 255, 230))
    elif face == "think":
        d.rounded_rectangle((470, 90, 630, 170), 40, fill=(255, 255, 255, 240), outline=LINE, width=6)
        for k in range(3):
            d.ellipse((505 + k * 40, 122, 521 + k * 40, 138), fill=LINE)
    return im.resize((W, H), Image.LANCZOS)


def _fit_image(path, base_dir):
    """立ち絵画像を 320×320 の枠に、縦横比を保って下寄せで収める"""
    p = path if os.path.isabs(path) else os.path.join(base_dir, path)
    im = Image.open(p).convert("RGBA")
    bbox = im.getbbox()
    if bbox: im = im.crop(bbox)
    k = min(W / im.width, H / im.height)
    im = im.resize((max(1, int(im.width * k)), max(1, int(im.height * k))), Image.LANCZOS)
    out = Image.new("RGBA", (W, H), (0, 0, 0, 0))
    out.alpha_composite(im, ((W - im.width) // 2, H - im.height))
    return out


def build_all(character_images=None, base_dir="."):
    """(キャラ, 表情, 口, 目) → 画像 の辞書を作る。
    character_images の例：{"komugi": {"base": "a.png", "mouth_half": "b.png", "mouth_open": "c.png",
                                        "eyes_closed": "d.png", "faces": {"smile": "e.png"}}}
    差分がない組み合わせは base（または faces の画像）で代用する。"""
    sp = {}
    for n in CHAR:
        imgs = (character_images or {}).get(n)
        cache = {}
        def load(key):
            if key and key not in cache: cache[key] = _fit_image(key, base_dir)
            return cache.get(key)
        for fc in FACES:
            for m in MOUTHS:
                for e in EYES:
                    if not imgs:
                        sp[(n, fc, m, e)] = draw(n, fc, m, e)
                        continue
                    key = imgs["base"]
                    if fc in imgs.get("faces", {}): key = imgs["faces"][fc]
                    elif e == "closed" and imgs.get("eyes_closed"): key = imgs["eyes_closed"]
                    elif m == "open" and imgs.get("mouth_open"): key = imgs["mouth_open"]
                    elif m in ("half", "open") and (imgs.get("mouth_half") or imgs.get("mouth_open")):
                        key = imgs.get("mouth_half") or imgs["mouth_open"]
                    sp[(n, fc, m, e)] = load(key)
    return sp


def sheet(path, sprites):
    """全表情を1枚に並べた確認用画像"""
    im = Image.new("RGBA", (W * 5, H * 4), (190, 225, 250, 255))
    for j, n in enumerate(CHAR):
        for i, fc in enumerate(FACES):
            im.alpha_composite(sprites[(n, fc, "closed", "open")], (i * W, j * 2 * H))
            im.alpha_composite(sprites[(n, fc, "open" if i % 2 == 0 else "half", "closed" if i == 4 else "open")], (i * W, (j * 2 + 1) * H))
    im.convert("RGB").save(path)
```

### render.py
```python
"""ゆっくり解説風動画を書き出す。
使い方：
  python3 render.py script.json 出力フォルダ --check   # 読み・改行・長さ・パネル文字の確認だけ（動画は作らない）
  python3 render.py script.json 出力フォルダ           # 動画、字幕、サムネイルを書き出す
作業ファイル（音声、確認用画像、時刻表）は ./work に置く。
"""
import importlib.util, json, math, os, random, re, subprocess, sys, time, wave
import numpy as np
import pyopenjtalk
from PIL import Image, ImageDraw, ImageFilter, ImageFont
import chars

SR, FPS, VW, VH = 48000, 30, 1920, 1080
TITLE_SEC, END_SEC, GAP, CH_GAP = 2.5, 4.0, 0.25, 0.6
VOICE = {"komugi": dict(half_tone=2, speed=1.10, k=1.06),     # 明るく幼い声
         "azuki": dict(half_tone=-4, speed=1.00, k=0.92)}     # 低く落ち着いた声
COLOR = {"komugi": (235, 120, 30), "azuki": (120, 70, 190)}  # 字幕の縁取り
POS = {"azuki": (50, VH - 312), "komugi": (VW - 370, VH - 312)}
PX0, PY0, PX1, PY1 = 160, 100, 1760, 720                      # 解説パネル
SX0, SX1, SY0, SY1 = 390, 1530, 790, 1040                     # 字幕
NO_START = set("、。，．！？!?）」』ー〜…ぁぃぅぇぉっゃゅょァィゥェォッャュョ")
PUNCT = "、。！？!?）」"
CREDIT = ["音声合成：Open JTalk", "音声モデル：HTS Voice \"Mei\"（© Nagoya Institute of Technology, CC BY 3.0）"]

args = [a for a in sys.argv[1:] if not a.startswith("--")]
CHECK = "--check" in sys.argv
SCRIPT, OUT = args[0], (args[1] if len(args) > 1 else "out")
WORK = "work"
os.makedirs(OUT, exist_ok=True); os.makedirs(WORK, exist_ok=True)
sc = json.load(open(SCRIPT, encoding="utf-8"))
lines = sc["lines"]
chapters = {c["id"]: c["title"] for c in sc["chapters"]}
col = {"bg_top": [120, 185, 240], "bg_bottom": [215, 238, 252], "accent": [40, 70, 130], "line": [90, 140, 200]}
col.update(sc.get("colors", {}))
col = {k: tuple(v) for k, v in col.items()}
FB, FK = chars.find_font("Bold"), chars.find_font("Black")
_fonts = {}
def font(size, black=False):
    key = (int(size), black)
    if key not in _fonts: _fonts[key] = ImageFont.truetype(FK if black else FB, int(size), index=0)
    return _fonts[key]
warnings = []

# 図（diagram）を描く関数：script.json と同じフォルダの diagrams.py の DIAGRAMS = {"名前": 関数(d, box, font)}
DIAGRAMS = {}
_dp = os.path.join(os.path.dirname(os.path.abspath(SCRIPT)), "diagrams.py")
if os.path.exists(_dp):
    _spec = importlib.util.spec_from_file_location("diagrams", _dp); _m = importlib.util.module_from_spec(_spec)
    _spec.loader.exec_module(_m); DIAGRAMS = _m.DIAGRAMS
for ln in lines:
    p = ln.get("panel")
    if p and p["type"] == "diagram" and p.get("name") not in DIAGRAMS:
        warnings.append(f"図 '{p.get('name')}' を描く関数が diagrams.py にありません")


# ---------- 改行 ----------
_bp_cache = {}
def break_points(text):
    """改行してよい位置：単語の区切りのうち、次の語が助詞・助動詞・記号・接尾辞・非自立語でない位置"""
    if text in _bp_cache: return _bp_cache[text]
    pts, off = set(), 0
    for seg in re.split(r"([0-9A-Za-z.,%+\-]+)", text):   # 英数字は解析で位置がずれるので分けて扱う
        if not seg: continue
        if re.fullmatch(r"[0-9A-Za-z.,%+\-]+", seg):
            if off > 0: pts.add(off)
        else:
            njd = pyopenjtalk.run_frontend(seg)
            if "".join(n["string"] for n in njd) == seg:
                pos = off
                for a, b in zip(njd, njd[1:]):
                    pos += len(a["string"])
                    if b["pos"] not in ("助詞", "助動詞", "記号") and b["pos_group1"] not in ("接尾", "非自立"):
                        pts.add(pos)
            else:
                pts.update(range(off + 1, off + len(seg)))
        off += len(seg)
    _bp_cache[text] = pts
    return pts


def wrap(text, f, maxw):
    """1行に入ればそのまま。入らなければ、文節の切れ目で2行に分ける（句読点の後を優先し、長さをそろえる）"""
    if f.getlength(text) <= maxw: return [text], True
    best, best_score, bps = None, 1e18, break_points(text)
    for i in range(2, len(text) - 1):
        a, b = text[:i], text[i:]
        if i not in bps or b[0] in NO_START: continue
        wa, wb = f.getlength(a), f.getlength(b)
        if max(wa, wb) > maxw: continue
        score = abs(wa - wb) - (f.size * 6 if a[-1] in PUNCT else 0)
        if score < best_score: best, best_score = [a, b], score
    if best: return best, True
    out, cur = [], ""
    for ch in text:
        if f.getlength(cur + ch) > maxw and cur and ch not in NO_START: out.append(cur); cur = ch
        else: cur += ch
    return out + ([cur] if cur else []), False


def fit(text, size, maxw, maxlines, black=False, one_line_min=None, prefer_punct=False, label=""):
    """文字を小さくしながら maxlines 行に収める。短い文は one_line_min まで縮めて1行を優先する"""
    if one_line_min:
        for s in range(size, one_line_min - 1, -4):
            if font(s, black).getlength(text) <= maxw: return font(s, black), [text]
    cands = []   # 文節の切れ目で収まった候補（大きい文字から）
    for s in range(size, int(size * 0.8) - 1, -4):
        f = font(s, black); ls, ok = wrap(text, f, maxw)
        if ok and len(ls) <= maxlines: cands.append((f, ls))
    if cands:
        if prefer_punct:   # 少し（15%まで）小さくすれば1行、または句読点で分けられるなら、そちらを選ぶ
            for f, ls in cands:
                if f.size >= size * 0.85 and (len(ls) == 1 or ls[0][-1] in PUNCT): return f, ls
        return cands[0]
    for s in range(int(size * 0.8), 19, -4):
        f = font(s, black); ls, ok = wrap(text, f, maxw)
        if len(ls) <= maxlines:
            warnings.append(f"{label}の文字が長すぎます（{s}pxまで縮小）：{text}"); return f, ls
    warnings.append(f"{label}が収まりません：{text}"); return f, ls[:maxlines]


# ---------- 音声 ----------
def synth(ln, i):
    v = VOICE[ln["speaker"]]
    x, sr = pyopenjtalk.tts(ln["yomi"], speed=v["speed"], half_tone=v["half_tone"])
    x = np.clip(x / 32768.0, -1, 1)
    raw, out = f"{WORK}/l{i:03d}_raw.wav", f"{WORK}/l{i:03d}.wav"
    with wave.open(raw, "wb") as w:
        w.setnchannels(1); w.setsampwidth(2); w.setframerate(sr)
        w.writeframes((x * 32767).astype(np.int16).tobytes())
    k = v["k"]
    subprocess.run(["ffmpeg", "-y", "-loglevel", "error", "-i", raw, "-af",
                    f"asetrate={int(sr * k)},aresample={SR},atempo={1 / k:.5f}", "-ac", "1", out], check=True)
    with wave.open(out) as w:
        y = np.frombuffer(w.readframes(w.getnframes()), np.int16).astype(np.float32) / 32768
    nz = np.where(np.abs(y) > 0.01)[0]                     # 前後の無音を詰める
    return y[max(nz[0] - 480, 0): nz[-1] + 480] if len(nz) else y


print("[読みの確認] 読み間違いがあれば yomi をひらがなに直す")
for i, ln in enumerate(lines):
    print(f"{i + 1:03d} {ln['speaker']:6s} {pyopenjtalk.g2p(ln['yomi'], kana=True)}")

t0 = time.time()
parts, t, prev_ch = [np.zeros(int(TITLE_SEC * SR), np.float32)], TITLE_SEC, None
for i, ln in enumerate(lines):
    y = synth(ln, i)
    if prev_ch is not None:
        g = CH_GAP if ln["chapter"] != prev_ch else GAP
        parts.append(np.zeros(int(g * SR), np.float32)); t += g
    ln["start"], ln["end"], ln["audio"] = t, t + len(y) / SR, y
    parts.append(y); t = ln["end"]; prev_ch = ln["chapter"]
parts.append(np.zeros(int(END_SEC * SR), np.float32))
full = np.concatenate(parts)
TOTAL = len(full) / SR
print(f"\n[長さ] {TOTAL:.1f}秒（{int(TOTAL // 60)}分{int(TOTAL % 60)}秒）、本文 {sum(len(l['text']) for l in lines)}字（音声合成 {time.time() - t0:.0f}秒）")

# パネルと字幕の文字を試しに組んで、収まるか確かめる
def sub_layout(text):
    return fit(text, 60, SX1 - SX0 - 80, 2, prefer_punct=True, label="字幕")

print("\n[字幕の改行]")
for i, ln in enumerate(lines):
    f, ls = sub_layout(ln["text"])
    print(f"{i + 1:03d} {f.size}px  {' ／ '.join(ls)}")

with wave.open(f"{WORK}/mix_raw.wav", "wb") as w:
    w.setnchannels(1); w.setsampwidth(2); w.setframerate(SR)
    w.writeframes((np.clip(full, -1, 1) * 32767).astype(np.int16).tobytes())
subprocess.run(["ffmpeg", "-y", "-loglevel", "error", "-i", f"{WORK}/mix_raw.wav",     # loudnorm は192kHzで出すので48kHzに戻す
                "-af", "loudnorm=I=-16:TP=-1.5:LRA=11", "-ar", str(SR), f"{WORK}/mix.wav"], check=True)
for ln in lines:                                                                    # 口パク用の音量
    y, n = ln["audio"], int(SR / FPS)
    rms = np.array([np.sqrt(np.mean(y[j:j + n] ** 2)) for j in range(0, len(y), n)])
    ln["rms"] = rms / (rms.max() + 1e-9)


# ---------- 画面 ----------
sprites = chars.build_all(sc.get("character_images"), os.path.dirname(os.path.abspath(SCRIPT)))
chars.sheet(f"{WORK}/characters.png", sprites)


def make_bg():
    g = np.linspace(0, 1, VH)[:, None]
    top, bot = np.array(col["bg_top"]), np.array(col["bg_bottom"])
    arr = (top * (1 - g) + bot * g)[:, None, :].repeat(VW, 1).reshape(VH, VW, 3)
    im = Image.fromarray(arr.astype(np.uint8)).convert("RGBA")
    ov = Image.new("RGBA", (VW, VH)); d = ImageDraw.Draw(ov); rnd = random.Random(1)
    for _ in range(40):
        x, y, r = rnd.randint(0, VW), rnd.randint(0, VH), rnd.randint(20, 90)
        d.ellipse((x - r, y - r, x + r, y + r), fill=(255, 255, 255, 28))
    return Image.alpha_composite(im, ov)


BG = make_bg()


def draw_panel(im, panel):
    ov = Image.new("RGBA", (VW, VH)); d = ImageDraw.Draw(ov)
    d.rounded_rectangle((PX0 + 8, PY0 + 12, PX1 + 8, PY1 + 12), 40, fill=(30, 60, 100, 60))
    ov = ov.filter(ImageFilter.GaussianBlur(10)); d = ImageDraw.Draw(ov)
    d.rounded_rectangle((PX0, PY0, PX1, PY1), 40, fill=(255, 255, 255, 240), outline=col["line"], width=6)
    cx, cw, tp, acc = (PX0 + PX1) // 2, PX1 - PX0 - 160, panel["type"], col["accent"]
    if tp in ("title", "end"):
        txt = sc["title"] if tp == "title" else "ご視聴ありがとうございました"
        f, ls = fit(txt, 110, cw, 2, True, one_line_min=88, label="タイトル")
        y = (PY0 + PY1) // 2 - len(ls) * f.size * 0.65 - (40 if tp == "end" else 0)
        for l in ls: d.text((cx, y), l, font=f, fill=acc, anchor="mt"); y += f.size * 1.3
        if tp == "end":
            for j, l in enumerate(CREDIT + sc.get("extra_credits", [])):
                d.text((cx, PY1 - 130 + j * 40), l, font=font(28), fill=(90, 100, 120), anchor="mt")
    elif tp == "heading":
        d.text((cx, PY0 + 150), f"第{panel['_ch']}章", font=font(56), fill=col["line"], anchor="mt")
        f, ls = fit(panel["heading"], 110, cw, 2, True, one_line_min=88, label="章見出し")
        y = PY0 + 260
        for l in ls: d.text((cx, y), l, font=f, fill=acc, anchor="mt"); y += f.size * 1.3
    elif tp == "bullets":
        f, ls = fit(panel["heading"], 76, cw, 1, True, label="箇条書きの見出し")
        d.text((cx, PY0 + 60), ls[0], font=f, fill=acc, anchor="mt")
        d.line((PX0 + 120, PY0 + 170, PX1 - 120, PY0 + 170), fill=col["line"], width=5)
        y = PY0 + 220
        for it in panel["items"][:4]:
            d.ellipse((PX0 + 150, y + 20, PX0 + 180, y + 50), fill=col["line"])
            f2, ls2 = fit(it, 62, cw - 120, 1, label="箇条書き")
            d.text((PX0 + 210, y), ls2[0], font=f2, fill=(50, 50, 60)); y += 110
    elif tp == "keyword":
        f, ls = fit(panel["keyword"], 150, cw, 1, True, label="キーワード")
        d.text((cx, PY0 + 170), ls[0], font=f, fill=(210, 80, 60), anchor="mt", stroke_width=4, stroke_fill=(255, 255, 255))
        f2, ls2 = fit(panel.get("sub", ""), 60, cw, 2, label="キーワードの補足")
        y = PY0 + 420
        for l in ls2: d.text((cx, y), l, font=f2, fill=(60, 60, 70), anchor="mt"); y += 80
    elif tp == "diagram":
        top = PY0 + 40
        if panel.get("heading"):
            f, ls = fit(panel["heading"], 56, cw, 1, True, label="図の見出し")
            d.text((cx, PY0 + 40), ls[0], font=f, fill=acc, anchor="mt"); top = PY0 + 130
        fn = DIAGRAMS.get(panel.get("name"))
        if fn: fn(d, (PX0 + 60, top, PX1 - 60, PY1 - 40), font)
        else: d.text((cx, (top + PY1) // 2), "（図）" + panel.get("spec", ""), font=font(40), fill=(200, 0, 0), anchor="mm")
    return Image.alpha_composite(im, ov)


def draw_label(im, ch):
    if ch is None: return im
    ov = Image.new("RGBA", (VW, VH)); d = ImageDraw.Draw(ov)
    txt, f = f"第{ch}章  {chapters[ch]}", font(36)
    d.rounded_rectangle((24, 22, 24 + f.getlength(txt) + 50, 82), 30, fill=col["accent"] + (230,))
    d.text((49, 52), txt, font=f, fill=(255, 255, 255), anchor="lm")
    return Image.alpha_composite(im, ov)


def draw_sub(ln):
    im = Image.new("RGBA", (VW, VH)); d = ImageDraw.Draw(im)
    d.rounded_rectangle((SX0, SY0, SX1, SY1), 30, fill=(20, 25, 40, 150))
    f, ls = sub_layout(ln["text"])
    lh = f.size * 1.35; y = (SY0 + SY1) / 2 - lh * len(ls) / 2 + 6
    for l in ls:
        d.text(((SX0 + SX1) / 2, y), l, font=f, fill=(255, 255, 255), anchor="mt", stroke_width=8, stroke_fill=COLOR[ln["speaker"]])
        y += lh
    return im


cur = {"type": "title"}
for ln in lines:
    if ln.get("panel"): cur = dict(ln["panel"]); cur["_ch"] = ln["chapter"]
    ln["_panel"] = cur
bases, subs = {}, {}
def base_for(panel, ch):
    key = (json.dumps(panel, ensure_ascii=False, sort_keys=True), ch)
    if key not in bases: bases[key] = draw_label(draw_panel(BG, panel), ch).convert("RGB")
    return bases[key]


rnd, blink = random.Random(7), {}
for n in chars.CHAR:
    s, bt = set(), rnd.uniform(1, 3)
    while bt < TOTAL:
        f0 = int(bt * FPS); s.update({f0, f0 + 1, f0 + 2}); bt += rnd.uniform(3, 5)
    blink[n] = s
last_face = {n: "normal" for n in chars.CHAR}


def frame_at(fi, li):
    t = fi / FPS
    if t >= TOTAL - END_SEC: panel, ch, ln = {"type": "end"}, None, None
    elif li < 0: panel, ch, ln = {"type": "title"}, None, None
    else: ln = lines[li]; panel, ch = ln["_panel"], ln["chapter"]
    frame = base_for(panel, ch).copy()
    speaking = ln is not None and ln["start"] <= t < ln["end"]
    if ln is not None and t < ln["end"] + GAP:
        if li not in subs: subs[li] = draw_sub(ln)
        frame.paste(subs[li], (0, 0), subs[li])
    if ln is not None: last_face[ln["speaker"]] = ln.get("face", "normal")
    for n in ("azuki", "komugi"):
        mouth, dy = "closed", 0
        if speaking and ln["speaker"] == n:
            r = ln["rms"][min(int((t - ln["start"]) * FPS), len(ln["rms"]) - 1)]
            mouth = "open" if r > 0.45 else "half" if r > 0.15 else "closed"
            dy = -int(abs(math.sin(2 * math.pi * 2.2 * (t - ln["start"]))) * 12)
        eyes = "closed" if fi in blink[n] else "open"
        face = "smile" if t >= TOTAL - END_SEC else last_face[n]
        sp = sprites[(n, face, mouth, eyes)]
        frame.paste(sp, (POS[n][0], POS[n][1] + dy), sp)
    return frame


def save_sheet(frames, path):
    cols = 3; rows = math.ceil(len(frames) / cols)
    sheet = Image.new("RGB", (640 * cols, 360 * rows), "white")
    for i, im in enumerate(frames): sheet.paste(im, ((i % cols) * 640, (i // cols) * 360))
    sheet.save(path)


nframes = int(math.ceil(TOTAL * FPS))
# 確認する場面：タイトル、パネルが切り替わるセリフの途中、エンドカード
check_at = sorted({int(1.0 * FPS)} | {int((l["start"] + l["end"]) / 2 * FPS) for l in lines if l.get("panel")} | {nframes - int(2 * FPS)})

if CHECK:   # 動画は作らず、確認用画像だけ作る
    li, frames = -1, []
    for fi in check_at:
        while li + 1 < len(lines) and fi / FPS >= lines[li + 1]["start"]: li += 1
        frames.append(frame_at(fi, li).resize((640, 360)))
    save_sheet(frames, f"{WORK}/check.png")
    print("\n[注意]" if warnings else "\n[注意] なし")
    for w in warnings: print(" -", w)
    print(f"確認用画像：{WORK}/check.png（主な場面）、{WORK}/characters.png（キャラクターの全表情）")
    sys.exit(0)

# ---------- 書き出し ----------
video = os.path.join(OUT, "video.mp4")
ff = subprocess.Popen(["ffmpeg", "-y", "-loglevel", "error", "-f", "rawvideo", "-pix_fmt", "rgb24", "-s", f"{VW}x{VH}",
                       "-r", str(FPS), "-i", "-", "-i", f"{WORK}/mix.wav", "-c:v", "libx264", "-preset", "veryfast",
                       "-crf", "20", "-pix_fmt", "yuv420p", "-c:a", "aac", "-b:a", "192k", "-shortest",
                       "-movflags", "+faststart", video], stdin=subprocess.PIPE)
li, t1, check_frames, check_set = -1, time.time(), [], set(check_at)
for fi in range(nframes):
    while li + 1 < len(lines) and fi / FPS >= lines[li + 1]["start"]: li += 1
    fr = frame_at(fi, li)
    if fi in check_set: check_frames.append(fr.resize((640, 360)))
    ff.stdin.write(fr.tobytes())
    if fi % (FPS * 30) == 0: print(f"  書き出し中 {fi / FPS:.0f}/{TOTAL:.0f}秒", flush=True)
ff.stdin.close(); ff.wait()
print(f"[書き出し] {video}（{time.time() - t1:.0f}秒）")

save_sheet(check_frames, f"{WORK}/check.png")

# 字幕（SRT）、時刻表、チャプター
def ts(s, srt=True):
    h, m, sec = int(s // 3600), int(s % 3600 // 60), s % 60
    return f"{h:02d}:{m:02d}:{int(sec):02d},{int((sec % 1) * 1000):03d}" if srt else (f"{h}:{m:02d}:{int(sec):02d}" if h else f"{m}:{int(sec):02d}")
with open(os.path.join(OUT, "subtitles.srt"), "w", encoding="utf-8") as f:
    for i, ln in enumerate(lines): f.write(f"{i + 1}\n{ts(ln['start'])} --> {ts(ln['end'])}\n{ln['text']}\n\n")
json.dump([{"speaker": l["speaker"], "text": l["text"], "start": round(l["start"], 2), "end": round(l["end"], 2)} for l in lines],
          open(f"{WORK}/timeline.json", "w", encoding="utf-8"), ensure_ascii=False, indent=1)
with open(f"{WORK}/chapters.txt", "w", encoding="utf-8") as f:
    seen = []
    for ln in lines:
        if ln["chapter"] not in seen:
            seen.append(ln["chapter"])
            f.write(f"{'0:00' if len(seen) == 1 else ts(ln['start'], False)} {chapters[ln['chapter']]}\n")

# サムネイル（1280×720）
th = BG.convert("RGB").resize((1280, 720)).convert("RGBA"); d = ImageDraw.Draw(th)
f, ls = fit(sc.get("thumbnail_text", sc["title"]), 120, 1160, 2, True, label="サムネイル")
y = 70
for l in ls: d.text((640, y), l, font=f, fill=(255, 255, 255), anchor="mt", stroke_width=12, stroke_fill=col["accent"]); y += f.size * 1.25
for n, x, fc in [("azuki", 120, "smile"), ("komugi", 760, "surprise")]:
    th.alpha_composite(sprites[(n, fc, "open", "open")].resize((400, 400), Image.LANCZOS), (x, 330))
th.convert("RGB").save(os.path.join(OUT, "thumbnail.png"))

print("[注意]" if warnings else "[注意] なし")
for w in warnings: print(" -", w)
print(f"完了：{TOTAL:.1f}秒。確認用画像 {WORK}/check.png と {WORK}/characters.png を見ること")
```
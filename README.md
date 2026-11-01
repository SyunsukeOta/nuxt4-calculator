# 画面遷移
- ![標準](app/assets/readme/電卓画像.png)

# 必要な機能の洗い出し(11/1-17:20)
- 誰が: 顧客
- 何を: 数式を計算
- どのように: 数式を入力
- 入力: 数値、演算子、「=」
- 出力: 計算結果
- ゴール: 2 つの数値と 1 つの演算子を使用して計算

# ボタン(11/1-18:00)
- NumberButton: 濃い灰色
  - 数字: 0~9
  - 小数点: 「.」
- FunctionButton: 薄い灰色
  - 符号切替: 「+/-」
  - Delete: 「x」
  - Clear: 「AC」/「C」
- OperatorButton: オレンジ色
  - 演算子: 「＋」「ー」「×」「÷」
  - 等号: 「＝」

# Claude + Figmaデザイン実装(11/1-18:00)
## 1. カラー登録（Variables）
- 画像のピクセルから色を抽出（Display P3 → sRGB に変換）
- `Primitives`: 色の実値（`black` `white` `gray/400〜950` `orange/300・500`）
- `Color`: 用途別に Primitives を参照
  - 背景: `color/bg/app`
  - ボタン: `color/button/{number,function,operator}/{bg,border}`
  - 文字: `color/text/{primary,secondary}`
- `Colors` フレームに全色のスウォッチを配置

## 2. ボタン（Components）
- `Buttons` フレームにマスターを 1 つずつ配置
  - `NumberButton`（0）/ `FunctionButton`（AC）/ `OperatorButton`（+）
- 86×86 の円、色はすべて Variables に紐付け
- `Label` プロパティでラベルを切替

## 3. 画面
| フレーム | 元画像 | 表示 | AC/C |
|---|---|---|---|
| `Calculator` | 電卓画像.png | 0 | AC |
| `CalculatorResult` | 電卓計算結果.png | 12+5 → 17 | AC |
| `CalculatorTyping` | 電卓入力中.png | 1 | C |

```
Calculator（384×835）
├── Display … 残りの高さを使い、右下揃え
│   ├── Expression（計算式・グレー／不要時は非表示）
│   └── Value（値・白）
└── Keypad … 5行×4列、ボタンのインスタンス（82×82）
```

## 4. ルール
- 新しい色は Variables に登録してから使う
- Display は `Expression` + `Value` の 2 段構成で統一
- ボタンはインスタンスの `Label` を書き換えて使う

## 5. 注意点
- フォントは Inter（SF Pro は Figma の API で文字幅を計算できなかった）
- 演算子は文字のため、画像のアイコンより細く小さい
- 上部の時計・計算機アイコンとステータスバーは対象外


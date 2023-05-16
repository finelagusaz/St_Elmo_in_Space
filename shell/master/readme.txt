# パーツ説明
## 本体
### 通常
- surface0.png
- surface0b.png
- surface0c.png

### 首傾げ
- surface0d.png
- surface0e.png
- surface0f.png

## 眼a
### 通常
- 0通常、1半目、2逸らし、3逸らし半目、4閉じ、5笑顔、6驚き、7にへ、8＞＜
### 首傾げ
- 20通常、21半目、22逸らし、23逸らし半目、24閉じ、25笑顔、26驚き、27にへ、28＞＜

## 眉b
### 通常
- 0通常、1下げ、2上げ
### 首傾げ
- 20通常、21下げ、22上げ

## 口c
### 通常
- 0微笑、1小閉、2小開、3大開、4ワ、5∧
### 首傾げ
- 20微笑、21小閉、22小開、23大開、24ワ、25∧

## 顔d
- 0照れ、1首傾げ照れ

----

## 画像一覧

surface1000.png　目閉じ／素
surface1001.png　半目／素
surface1002.png　半目逸らし／素
surface1003.png　目閉じ／照れ
surface1004.png　半目／照れ
surface1005.png　半目逸らし／照れ
surface1010.png　首傾げ／目閉じ／素
surface1011.png　首傾げ／半目／素
surface1012.png　首傾げ／半目逸らし／素
surface1013.png　首傾げ／目閉じ／照れ
surface1014.png　首傾げ／半目／照れ
surface1015.png　首傾げ／半目逸らし／照れ

※surface画像の番号は仮当てです。必要に応じてずらしていただければと

## メモ

下記はsurface.txtのサンプルです。

// まばたき　通常（半目にならないパターン）
surface.append0
{
	animation2.interval,periodic,4
	animation2.pattern0,overlay,1001,0-800,0,0
	animation2.pattern1,overlay,1000,50,0,0
	animation2.pattern2,overlay,1001,50,0,0
	animation2.pattern3,overlay,-1,50,0,0
}

// まばたき　通常（半目になるパターン）
surface.append1
{
	animation2.interval,periodic,4
	animation2.pattern1,overlay,1000,0-800,0,0
	animation2.pattern3,overlay,-1,50,0,0
}

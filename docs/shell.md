# master シェルについて

## パーツ

### 本体

#### 通常

- surface0.png
- surface0b.png
- surface0c.png

#### 首傾げ

- surface0d.png
- surface0e.png
- surface0f.png

### 眼a

#### 通常

- 0通常、1半目、2逸らし、3逸らし半目、4閉じ、5笑顔、6驚き、7にへ、8＞＜

#### 首傾げ

- 20通常、21半目、22逸らし、23逸らし半目、24閉じ、25笑顔、26驚き、27にへ、28＞＜

### 眉b

#### 通常

- 0通常、1下げ、2上げ

#### 首傾げ

- 20通常、21下げ、22上げ

### 口c

#### 通常

- 0微笑、1小閉、2小開、3大開、4ワ、5∧

#### 首傾げ

- 20微笑、21小閉、22小開、23大開、24ワ、25∧

### 顔d

- 0照れ、1首傾げ照れ

## 瞬き

- surface1000.png　目閉じ／素
- surface1001.png　半目／素
- surface1002.png　半目逸らし／素
- surface1003.png　目閉じ／照れ
- surface1004.png　半目／照れ
- surface1005.png　半目逸らし／照れ
- surface1010.png　首傾げ／目閉じ／素
- surface1011.png　首傾げ／半目／素
- surface1012.png　首傾げ／半目逸らし／素
- surface1013.png　首傾げ／目閉じ／照れ
- surface1014.png　首傾げ／半目／照れ
- surface1015.png　首傾げ／半目逸らし／照れ

### サンプル

下記はsurface.txtのサンプルです。

```surfaces.txt
// まばたき　通常（半目にならないパターン）
surface.append0
{
	animation2.interval,periodic,4
	animation2.pattern0,overlay,1001,0-800,0,0
	animation2.pattern1,overlay,1000,50,0,0
	animation2.pattern2,overlay,1001,50,0,0
	animation2.pattern3,overlay,-1,50,0,0
}
```

```surfaces.txt
// まばたき　通常（半目になるパターン）
surface.append1
{
	animation2.interval,periodic,4
	animation2.pattern1,overlay,1000,0-800,0,0
	animation2.pattern3,overlay,-1,50,0,0
}
```

## 口パク

surface1100.png　口パク1
surface1101.png　口パク1
surface1102.png　口パク1
surface1110.png　首傾げ／口パク1
surface1111.png　首傾げ／口パク2
surface1112.png　首傾げ／口パク3

```surfaces.txt
surface0-9,11,12
{
	element0,base,surface0.png,0,0
	
	animation100.interval, talk
	animation100.pattern0, overlay, 1100, 100, 0, 0
	animation100.pattern1, overlay, 1101, 100, 0, 0
	animation100.pattern2, overlay, 1102, 100, 0, 0
	animation100.pattern3, overlay, 1100, 100, 0, 0
	animation100.pattern4, overlay, -1, 80, 0, 0
}

surface200-209,211,212
{
	element0,base,surface0b.png,0,0
	
	animation100.interval, talk
	animation100.pattern0, overlay, 1100, 100, 0, 0
	animation100.pattern1, overlay, 1101, 100, 0, 0
	animation100.pattern2, overlay, 1102, 100, 0, 0
	animation100.pattern3, overlay, 1100, 100, 0, 0
	animation100.pattern4, overlay, -1, 80, 0, 0
}

surface300-309,311,312
{
	element0,base,surface0c.png,0,0
	
	animation100.interval, talk
	animation100.pattern0, overlay, 1100, 100, 0, 0
	animation100.pattern1, overlay, 1101, 100, 0, 0
	animation100.pattern2, overlay, 1102, 100, 0, 0
	animation100.pattern3, overlay, 1100, 100, 0, 0
	animation100.pattern4, overlay, -1, 80, 0, 0
}

surface400-409,411,412
{
	element0,base,surface0d.png,0,0
	
	animation100.interval, talk
	animation100.pattern0, overlay, 1110, 100, 0, 0
	animation100.pattern1, overlay, 1111, 100, 0, 0
	animation100.pattern2, overlay, 1112, 100, 0, 0
	animation100.pattern3, overlay, 1110, 100, 0, 0
	animation100.pattern4, overlay, -1, 80, 0, 0
}

surface500-509,511,512
{
	element0,base,surface0e.png,0,0
	
	animation100.interval, talk
	animation100.pattern0, overlay, 1110, 100, 0, 0
	animation100.pattern1, overlay, 1111, 100, 0, 0
	animation100.pattern2, overlay, 1112, 100, 0, 0
	animation100.pattern3, overlay, 1110, 100, 0, 0
	animation100.pattern4, overlay, -1, 80, 0, 0
}

surface600-609,611,612
{
	element0,base,surface0f.png,0,0
	
	animation100.interval, talk
	animation100.pattern0, overlay, 1110, 100, 0, 0
	animation100.pattern1, overlay, 1111, 100, 0, 0
	animation100.pattern2, overlay, 1112, 100, 0, 0
	animation100.pattern3, overlay, 1110, 100, 0, 0
	animation100.pattern4, overlay, -1, 80, 0, 0
}
```

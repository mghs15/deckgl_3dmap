# deckgl_3dmap
地理院地図Vectorのデータを加工して、deck.glで3Dっぽく表示するサンプル。

高さ情報は近傍の等高線から取得。あくまで見栄えを重視して加工・表示処理をしているため、高さ情報は信用できないことに注意。

## 3Dサンプル
### 任意の場所
表示している範囲のデータを3Dで表示する。
* https://mghs15.github.io/deckgl_3dmap/onthefly/index.html

※データの処理も行うため、範囲が広いとPC、ブラウザの処理が追いつかない場合もあるので注意。


### 筑波山周辺
* 等高線で地形を表現 https://mghs15.github.io/deckgl_3dmap/tsukuba/index.html
* 標高タイルで地形を表現 https://mghs15.github.io/deckgl_3dmap/tsukuba/terrain.html


※3Dデータは建物（標高値含む）のみ、ZL15のベクトルタイルから取得。他はZL14から。


### 然別駅周辺
* 等高線で地形を表現 https://mghs15.github.io/deckgl_3dmap/shikaribetsu/index.html
* 標高タイルで地形を表現 https://mghs15.github.io/deckgl_3dmap/shikaribetsu/terrain.html


※3DデータはすべてZL15のベクトルタイルから取得。

### 九頭竜湖駅周辺
* 等高線で地形を表現 https://mghs15.github.io/deckgl_3dmap/kuzuryuko/index.html
* 標高タイルで地形を表現 https://mghs15.github.io/deckgl_3dmap/kuzuryuko/terrain.html


※3DデータはすべてZL15のベクトルタイルから取得。

## データの作成
地理院地図Vectorのベクトルタイルから必要なデータを読み込んだあと、近傍の等高線の値から標高データを取得する。
Mapbox GL JSとTurf.jsを用いて、ブラウザ上で処理を行う。大量のデータを処理するため、表示範囲には注意。

Mapbox GL JSの`querySourceFeatures`を利用しているため、タイル単位でデータが取得される。

### まとめて作成
任意の位置でデータを作成。基本的には、以下の各ツールをベースにしたもの。
MultiLineStinrg、MultiPolygonにも対応している。

（上記3Dサンプルの「任意で表示」と同じもの。）

* まとめて https://mghs15.github.io/deckgl_3dmap/onthefly/index.html

### 個別に処理
#### 等高線
データに含まれる"alti"属性を使用。

* 等高線 https://mghs15.github.io/deckgl_3dmap/getGeoJSON/getgeojson_contour.html

#### ラインデータ
LineStringの頂点ごとに最近傍の等高線頂点を探し、その"alti"属性をLineRtringの頂点に付与。MultiLineStinrgには対応していない。

* 鉄道 https://mghs15.github.io/deckgl_3dmap/getGeoJSON/getgeojson_railway_alti.html
* 道路 https://mghs15.github.io/deckgl_3dmap/getGeoJSON/getgeojson_road_alti.html

#### ポリゴンデータ
Polygonの代表点を計算し、その代表点と最近傍の等高線頂点を探し、その"alti"属性をPolygon属性値として付与。MultiPolygonには対応していない。

* 建物 https://mghs15.github.io/deckgl_3dmap/getGeoJSON/getgeojson_building_alti.html
* 水域 https://mghs15.github.io/deckgl_3dmap/getGeoJSON/getgeojson_waterarea_alti.html


## 参考にした資料
### データ
* 国土地理院 地理院地図Vector https://maps.gsi.go.jp/vector/
* 国土地理院 標高タイル https://maps.gsi.go.jp/development/hyokochi.html

### 利用ライブラリ
* deck.gl https://deck.gl/
* Mapbox GL JS https://docs.mapbox.com/mapbox-gl-js/api/
* Turf.js https://turfjs.org/

### ドキュメント等
* Slippy map tilenames https://wiki.openstreetmap.org/wiki/Slippy_map_tilenames
* 国土地理院 標高を求めるプログラム https://maps.gsi.go.jp/development/elevation.html
* MDN web docs 画像を使う https://developer.mozilla.org/ja/docs/Web/Guide/HTML/Canvas_tutorial/Using_images



# 機能要件
* 鉄道網は、複数の都市（駅）・複数の線路区間で構成されている。
* 都市間のルートを決定するための経路探索アルゴリズムを実装すること。
* ２都市間の平均移動時間を推定すること。
* 列車同士の衝突はしない <= TODO: 考慮しないといけないかも...。
* ランダムイベントジェネレータを実装すること。 <= 何これ？ <= 多分、人が入ったり、事故ったりするやつ…かな？

列車には以下の情報を含めること
 - 名前
 - 一意の識別子（ID）
 - 最高加速力
 - 最大ブレーキ力

線路区間には以下の情報を含めること
 - 最高速度
 - つながっている都市

# 非機能要件

デザインパターンの使用。（学習のために３つ以上必要）
UML図（学習のため少なくとも２種類の図が必要）
構造とクラスを巧みにカプセル化する必要がある。

以下の原則に従うこと
 - SOLID
 - KISS
 - DRY

# 流れ
## メイン
1. [鉄道網ファイル], [列車ファイル]を受け取る
1. 最適な経路を計算し、[結果ファイル]を出力する

## ボーナス
1. リアルタイムシミュレートを行う

# ファイル形式
アプリケーションが正しい数の引数を受け取らなかった場合は、実行ファイルの使用方法を説明する使用方法を出力してください。また、入力ファイルの作成方法を出力する--helpも追加してください。

## 鉄道網ファイル

各行に以下のいずれかのデータが入力されている。

### 駅データ
 - 名前
```
Node City名前
```

### レールノード
 - 名前
```
Node RailNode名前
```

### レール（区間）データ
 - 端点1
 - 端点2
 - レールの長さ(km)
 - 区間制限速度(km/h)
```
Rail 端点１ 端点２ レールの長さ 区間制限速度
```

### 例：
```
Node CityA
Node CityB
Node CityC
Node RailNodeA
Node RailNodeB
Node RailNodeC
Node RailNodeD
Node RailNodeE
Node RailNodeF
Node RailNodeG
Rail CityA RailNodeA 15.0 250.0
Rail RailNodeA RailNodeB 20.0 250.0
Rail RailNodeB RailNodeC 13.0 175.0
Rail RailNodeC CityB 5.0 150.0
Rail CityB RailNodeD 7.0 150.0
Rail RailNodeD RailNodeE 12.0 200.0
Rail RailNodeE CityC 6.0 150.0
Rail CityA RailNodeF 6.0 150.0
Rail RailNodeF RailNodeG 17.0 250.0
Rail RailNodeG CityC 17.0 250.0
Rail RailNodeA RailNodeD 23.02 250.0
Rail RailNodeA RailNodeE 19.0 250.0
Rail RailNodeA RailNodeG 6.7 150.0
Rail RailNodeB RailNodeD 20.24 225.0
```

## 列車ファイル
各行に以下のデータが入力される
1. 列車名
1. 重さ (m/t)
1. 摩擦係数
1. 最大加速力(kN)
1. 出発駅
1. 到着駅
1. 出発時刻
1. 各駅での停車時間
```
列車名 重さ 摩擦係数 最大加速力 出発駅 到着駅 出発時刻 各駅での停車時間
```

### 例
```
TrainAB 80 0.05 356.0 30.0 CityA CityB 14h10 00h10
TrainAC 60 0.05 412.0 40.0 CityA CityC 14h20 00h10
TrainBA 40 0.05 356.0 30.0 CityA CityB 14h24 00h10
```

## 結果ファイル
ファイル名："TrainName_TrainDepartureTime.result"
ヘッダーとして以下のデータが入力される

1. 列車名
1. 推定到着時間
```
Train : 列車名
Final travel time : 時間
...
```
**例**
```
Train : TrainAB1
Final travel time : 01h30m
...
```

ヘッダーに続き、各行に以下のデータが入力される
1. 開始時からのタイムスタンプ
1. 出発点
1. 到着点
1. 最終到着地点までの残りの距離
1. 何をしているか(行動)
1. 出発点から到着点までの距離をグラフで表示したもの(グラフ)
```
[タイムスタンプ] - [出発点][到着点] - [残りの距離] - [行動] - グラフ
...
```
**例**
```
[00h00]- [ CityA][RailNodeA]- [53.00km]- [Speed up]- [x][ ][ ][ ... ][ ][ ][ ]
[00h05]- [ CityA][RailNodeA]- [52.50km]- [Maintain]- [x][ ][ ][ ... ][ ][ ][ ]
[00h10]- [ CityA][RailNodeA]- [51.00km]- [Maintain]- [x][ ][ ][ ... ][ ][ ][ ]
...
...
[XXhXX]- [ CityA][RailNodeA]- [38.01km]- [Maintain]- [ ][ ][ ][ ... ][ ][ ][x]
[XXhXX]- [RailNodeA][RailNodeB]- [30.00km]- [Maintain]- [x][ ][ ][ ... ][ ][ ][ ][ ][ ][ ][ ][ ]
...
[XXhXX]- [RailNodeC][ CityB]- [01.00km]- [ Braking]- [ ][ ][ ][x][ ]
[XXhXX]- [RailNodeC][ CityB]- [00.90km]- [ Braking]- [ ][ ][ ][x][ ]
...
[XXhXX]- [RailNodeC][ CityB]- [00.00km]- [ Stopped]- [ ][ ][ ][ ][x]
```

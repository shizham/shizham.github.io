# Pythonからはじめるアルゴリズムトレードのメモ

[Github: Python for Algorithmic Trading](https://github.com/yhilpisch/py4at)

2022-04-08
## Chapter 2
* mini conda をパッケージマネージャーおよび仮想環境として効率的な方法
* Dockerを用いて mini conda なども設定可能．
* クラウドインスタンスは[DigitalOcean](https://www.digitalocean.com/)がよいらしい．

## Chapter 3
* HDFstoreによるデータの高速保存と読み込み
* 追加検索可能はto_hdf()メソッド
* バックテストで有意義らしい．

## Chapter 4
* ベクトルはリストでなく，NumPyによるn次元配列が対応
* data['return'] はnpでlogリターンを計算．data['strategy']はdata['position']をshift(1)してリターンとの積
* グロスリターン（累積リターン）は.apply(np.exp)．
* max DDやDDの最長時間なども可視化できる．
* モメンタムは執行コストとの戦い．比較的に長い足がベター(Moskwiz, et. al. 2012)
* 平均回帰はおそらくボラの時間的な変化が安定的なものにワーク．
* 回帰についての予想について．正解率が高ければいいうものでない．最悪の日から逃れ，最高の日で予想できることが肝．
  * タイミングの問題とあるが，賭ける資産がよく動くときに予想が当たるかどうか？という問題
  * 賭ける資産を予想するでなく，リターンを予想する．つまり，リターン分布におけるtailにおける正答率との関係．
  * 価格は直前に説明力があるが，リターンは全く異なってほぼない．なにせ過去N期間の価格の関数になっているから．
  * リターンは正規分布と単純化すればトリガーによる戦略は下から （トリガー超えにおける正解率）× （トリガー超えのtail確率）で下に有界．
  * したがってボラが大きいことはプラスに作用し，大事なことは比較的に大きめの変動のときの正解率．
* DNNはkreasとTensorFlowで簡単に実装できる．
  * 過去のlagを用いて対数リターンを予想させる．つまり，先っぽの確率にウエイトがかかり，そこを予想をしている
  * OOSでも最初にまごつくとある．これは十分な期間のリターンを予想しているから当然．（虎穴に入らずんば虎子を得ず）
  * OOSでも優位性がありそうにみえる．lagだけでなく特徴量を込みすると改善する

---

2022-04-16
# M1 + miniforge -> python env. 
Pythonの環境設定はM1の場合，全般的に癖がある．とても参考になったのは

[【M1 Pro/Max対応】M1 Mac環境構築ベストプラクティス](https://qiita.com/c60evaporator/items/aef6cc1581d2c4676504)

おかげで↓できるようになった
* 環境の設定と使用をコマンドベースで実行．
  * vscodeでpathを通してjupyterをvscodeで走らせることも．
  * git clone を用いて環境を揃えてquick startなど．
  * numeraiはlightgbmは(forge)condaを叩いて出ないとインストールできずマニュアルで行った．
  * Dockerなるものをざっくりと理解し，それから環境設定をvscode周りで展開

* Chapter 6までコードも追う．自分用に環境のメモやバージョンによる書き換えコードなど; 

```
conda create -n algo python=3.9
conda activate algo
git clone https://github.com/yhilpisch/py4at.git 
# to /Users/ttsuchiya/py4at
conda install numpy # start from Ch4
conda install pandas
conda install matplotlib
conda install scipy
conda install scikit-learn

# tf のインストール
conda install -c apple tensorflow-deps
conda install -c apple tensorflow-deps==2.6.0
python -m pip install tensorflow-metal

# 以下は書き換え
# https://stackoverflow.com/questions/62707558/importerror-cannot-import-name-adam-from-keras-optimizers

#from keras.optimizers import Adam
from tensorflow.keras.optimizers import Adam


# 昔のmethodみたい．https://teratail.com/questions/358850
# 以下を参考
# https://stackoverflow.com/questions/68836551/keras-attributeerror-sequential-object-has-no-attribute-predict-classes

# pred = model.predict_classes(training_data_[cols])
predict_x=model.predict(training_data_[cols]) 
pred =np.argmax(predict_x,axis=1)


# pred = model.predict_classes(test_data_[cols])
predict_x=model.predict(test_data_[cols]) 
pred =np.argmax(predict_x,axis=1)
```

---
2022-04-19

## Chapter 4 code
[Chapter 4 code](https://colab.research.google.com/drive/17CpQwKYyF-d5ZyDQLtg61lufA44rnBaH)
いろいろ書いたのに間違って消してしまった．．．
* 単純な戦略ではparameterのオーバーフィットになりかねない．

## Chapter 5 code
[Chapter 5 code](https://colab.research.google.com/drive/16lQZHd8m6gkJoLUv6voLjRZsK62Ctl6r#scrollTo=eJI1IjTP7wbJ)

* lagを使って予想する．線形回帰問題にすり替える．値を求めるのは難しいので符号，つまり分類問へ．
* Logisticは判断が難しい（合格ラインギリギリで合格したものと落ちたものにひっぱられる）ところに非線形構造を入れたもの．transaction cost入れても悪くはない．
* DNNでやる．model.fitで統一的に書ける．
```
set_seeds()
model = Sequential()
model.add(Dense(64, activation='relu',
        input_shape=(lags,)))
model.add(Dense(64, activation='relu'))
model.add(Dense(1, activation='sigmoid')) # <5>
model.compile(optimizer=optimizer,
              loss='binary_crossentropy',
              metrics=['accuracy'])
```
 * 適当にデータをこしらえてvalidationしても悪くなさそう．
 * 異なる特徴量，moment，ヴォラ，distanceを入れるとかなり強くなる．
 
 ## Chapter 10 code
 [Chapter 10 code](https://colab.research.google.com/drive/189PsD2FD25tR8UnCuHuOMr4C2BzqVv-Q#scrollTo=4AGKJyIUW77L)
 * ケーリー基準の説明と実験はここの章
 * transaction cost込みでも種々の特徴量を入れてMLすると強い．


 


### To do
- [x] dockerを用いて上のmini conda環境を揃えてみる. 2022.04.22.
- [ ] DropletsについてRSA公開鍵と秘密鍵を作成してセキュア環境の設定
- [ ] シンプルなlagと特徴量でNNしてみるとnumeraiやcryptはどうなるだろうか．


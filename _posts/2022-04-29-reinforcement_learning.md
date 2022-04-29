# Motivation

- [ ] 売買するアセット自体のEDAが欲しいところ．特にCryptが抜けている．特にBTC, ETHやそれ以外のCryptを込めた解析．
- [ ] [MPT](https://ja.wikipedia.org/wiki/%E7%8F%BE%E4%BB%A3%E3%83%9D%E3%83%BC%E3%83%88%E3%83%95%E3%82%A9%E3%83%AA%E3%82%AA%E7%90%86%E8%AB%96)は
リスクを抑えることが最適戦略としている．したがってもしモメンタムなどあれば埋もれてしまったリワードを取りこぼしている可能性がある．基準としては良さそうなのでMPTはベンチマークくらいでみる．
すると強化学習などによるアルファ探求が考えられるはず．例えば一定期間のリターン（たぶんtailが効く）をgainとした強化学習をまわしてアウトパフォームするか？

## クラスター分析と銘柄選出

たくさんの資産があっても似たようなものであったり，市場が異なることでまったく独特なものもある．これらは特有の性質を内在しており，収益の源泉を見出す契機となる．
ここでは，その特徴性について上手く分解する鍵がないかを考える．

<img width="849" alt="image" src="https://user-images.githubusercontent.com/102901806/165871911-a5bdb4dd-ebaf-42f5-9653-1048c6fbe9a0.png">

[上の図link](http://ir.hit.edu.cn/~jguo/docs/notes/report-in-princeton-research.pdf)

アイディアとしては上図のように各変数への到達を行列で表現して，コネクションが多いものを同種の資産と見做せると思う．
その際に各資産などは確率変数として，多次元のガウスと仮定して調べる．
<img width="184" alt="image" src="https://user-images.githubusercontent.com/102901806/165872223-bccbbc67-1ab1-4882-8be2-00bf688bdc36.png">

共分散逆行列の計算が必要になる．決定するのにはL1正則項を付与して求める．

<img width="949" alt="image" src="https://user-images.githubusercontent.com/102901806/165872629-242bf90a-6cbb-4105-8e5b-261f0e66085f.png">

[上の図にあるように疎な行列だからとんがった方が求めやすい](http://ir.hit.edu.cn/~jguo/docs/notes/report-in-princeton-research.pdf)

![image](https://user-images.githubusercontent.com/102901806/165872798-d73372a4-5c90-45c0-b90e-440d5fd67787.png)

[scikit-learn 1.0.2. 2.6.3. Sparse inverse covariance](https://scikit-learn.org/stable/modules/covariance.html#sparse-inverse-covariance)
にもあるようにスパース（疎）な行列の決定にはL1正則化がワーク．ヒストリカルな相関よりも（ガウスなど仮定しているので）上手くいくこともある．

## 実際のイメージを超えて強化学習へ

だいたいイメージ通りなことをやってくれている，
[データ分析で振り返るマーケット2020 〜クラスター分析からポートフォリオ最適化まで〜](https://qiita.com/kenji_shinoda/items/ccf4f1366cef04231de9) ．
実直な方法だとこれから，特異性を直行化したポートフォリオを組む感じだろうけど，ここではもう少し捻る．

特異性をカテゴライズするのは確かに真理の探究めいていて素敵．だけどゴールは収益を伸ばすことなので，
そういったことも考慮しつつ，教科学習させてガラガラポンさせたい．

そういったわけで以下を読み進める，まずざっくり．


# ゼロから作るDeep learning 4 のメモ

一章のバンデット問題からスタートする．いわゆる，勝率がわからないいくつかのゲームズから，試してみて，最適なゲームを選ぶことを目的とする．
このことをリワードとして考え，リワードはインクリメンタルに決まる関数になる．
したがって，「（勝率が見えないけど）定常」な場合は貪欲法，もしくは遊びを入れたε貪欲法で良い戦略が選べる．

現実的な問題を考えたときに，定常性はあまり望めない．非定常性をいかに扱うかがポイント．標本平均をとるのは勝率が定数であることによる．
非定常であればαという忘却率のようなものを採用して，直近の情報を強く反映させるのがシンプルでいて強力．

二章ではマルコフ決定仮定による枠組みが説明されている．また政策πを選ぶのに直前の最適な戦略をとればいい，いわゆるベルマン原理．




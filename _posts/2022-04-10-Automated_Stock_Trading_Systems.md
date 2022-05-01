# Automated Stock Trading Systemsのメモ

割と初歩的だけど重要なことが書いてある．集約すれば
* 逆相関するトレードシステム」をアセットのように組み合わせればボラが低減してシャープや絶対リターンが増すというもの．
* ロング・ショートの戦略で言えば共通してボラが高いものを選んでいる．これはテイルの確率をあげたいことと整合する．


## データクリーニング

サバイバルバイアスによる弊害はコラムとしてあるがかなり本質な気がする．
* 上場廃止になった銘柄なども反映して前処理
* 株式分割などのよう価格が狂う可能性がある特別な事態への対処
* 配当は買いポジションなら保守的にないものとしてカウント
* 売り戦略なら配当支払いはあえていれてズレが小さく

## 基準となる指標の基準パラメーターの選択

最適化すればデータから最大の収益にするパラメーターを選びたいところだけど
それはまさにオーバーフィッティングになりえる．
パラメーターが少しばかり動いたとしても収益の質を維持しうるものを考える．

* SMAやATRはパラメーターをもつ．その基準の選び方は以下のようにする．
* パラメーターを動かして目的とするリターンやドローダウンが最頻度のようになるパラメーターを選ぶ
* こうすることでデータが変化してもリターンやドローダウンの結果がロバストになる

## アセットアロケーション

無相関もしくは逆相関する戦略をいくつか組み合わせる冥利がポイントになる．
各戦略に割り振る最大の資金枠を設定し，想定される損失を加味したリスク・エクスポージャーの考えなど．

## ロング戦略

* SMAから上昇トレンドになっている期間をあぶりだす
* ランキングで上位を選ぶことで高ボラを選ぶ．またトレーダブルであるために流動性に関する条件を課す．
* 下からシフトしていくfloor priceでアンワイド．

## ショート戦略
* 利幅が小さい短期トレードなので収益機会を増やせるだけ増やす
* RSIや数日，連続して終値更新などの条件を課す．
* ランキングで下落が大きいものを選ぶことで高ボラを選ぶ．またトレーダブルであるために流動性に関する条件を課す．
* 執行を睨んでLimit orderを+4%として指す．Stop lossは3ATRなどでおいておく．

## 一般化すると

基本的なアイディアは
1. エントリーポイントとその後の値動きを直近のデータセットによる指標で予想
2. ボラが高いことを想定して，執行ルールをおく
3. ロングとショートは並行して走ることもあるが，ほぼ無相関，ちょっと逆相関くらい．

したがって，賭ける割合をコントロールすればボラが小さくなる．
さらにほぼ無相関だから収益は非線形に増加しうる．


# Relative Strength Strategies

[Meb Faber - Relative Strength Strategies for Investing - SSRN](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=1585517)

[Relative Strength Strategies](https://colab.research.google.com/drive/1mKpapSlPBgDx5AKc2cjCexB4AbJKAnZU?hl=ja#scrollTo=GtXFRIGUtGjc)

* ランキングソートを行う．例えば 3,6,12mのリターンを計算し，順位をつけ，上位3つを選出
* フィルターとして，12SMAより上である条件のもとでソートして上記3つのうちで満たすものに均等配分

## ポートフォリオ選出について実験

![image](https://user-images.githubusercontent.com/102901806/166391106-f3a07a01-23d5-4304-9344-71c90bc67bb6.png)

![image](https://user-images.githubusercontent.com/102901806/166391115-d2f27ea7-9caa-47a9-afc3-7c633e1b7a1b.png)


![image](https://user-images.githubusercontent.com/102901806/166391163-f1f9cba5-fc2e-4431-ad70-40be5c2969ba.png)

* コロナ相場の米株の強さはまさに異常そのもの．直近で弱くなると優位性が見られる．
* 細かい粒でやればやるほど収益機会が増すので**コスト（執行）との戦い**になりそう．

* 以下は個人的に開眼した点など
    * 判定条件などはdfでいっきに作って（ベクトル化）しまって時系列をあわせるために~shift(-1)すればよい~逆だわ．リークなのでshift(1)
    * df3.valuesメソッドで渡せば``` df4 = df2.mul(df3.values, 1)```，成分ごとの積が計算できてmean(axis=1)でリターンが求められる．

このほうが直感的に馴染むし，なにせ早い．コードのはじめはこういうことにいちいち時間を喰われてしまう．
けれど出来るようになれば悩んで試みた色々は応用の際の糧になるので加速度ついてできるようになっているはず．

## 執行戦略

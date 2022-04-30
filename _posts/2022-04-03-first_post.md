# Hello world

This is my first post

## 数式

```
# Set this to true to get LaTeX math equation support
use_math:
```

とあるので ``` true  ``` にしたが何も起こらない．はて．

よくわからないので以下を参考に書き出しはできた．大きさとか色とかちょっと難あり．


<img src="https://render.githubusercontent.com/render/math?math=e^{i \pi} = -1">


[A hack for showing LaTeX formulas in GitHub markdown.md](https://gist.github.com/a-rodin/fef3f543412d6e1ec5b6cf55bf197d7b)

## fastpagesに移行した方がいいかも

githubで完結しようとするとlatexなど面倒が起きている．
Jupyterやmarkdown (vscode)などに置き換えてやったほうが話が早いかも．

やってみたけれども
* colaboの数式の部分はpage内では表示されないらしい．
* colaboで連携してアップすると04-30が勝手に04_30になってしまって認識しない．

https://shizham.github.io/shizlog/

## なるほどeditorをつかうのか

$$ \int $$
で数式はpreviewでは見えないが投稿反映されるとみられる．
だからみんなvscodeなどを使って，それをpostするのね．



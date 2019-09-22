# coc.nvimとclangdを使ってROSプログラミングでの補完をする

## 結論

結論から言うと，`coc.nvim`でROSプログラミングの補完をするには，各パッケージのルートディレクトリ(`CMakeLists.txt`があるディレクトリ)に`compile_commands.json`ファイルを置く必要がある．

## `compile_commands.json`とは？

`clangd`が参照するファイル．ビルドオプションがこのファイルに記述され，それを基に`clangd`が補完を行う．

補完を行うにはこのファイルが必要となる．`CMakeLists.txt`に次の一文を記述すれば，このファイルを自動生成してくれる．

```
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```

ただし，生成される場所は，`~/ROS_WORKSPACE/build/MY_PACKAGE`(ROS_WORKSPACEはワークスペース名，MY_PACKAGEはパッケージの名前)であり，このままでは`clangd`は補完をしてくれない．
そこで，この生成されたファイルを自分のパッケージのルートディレクトリにコピーしてくる必要がある(シンボリックリンクでも多分行けるけど試してない)．

こうすることで，晴れて入力補完をしてくれるようになる．

# coc.nvimのコマンドメモ

## Extentionのインストール

neovimを起動し，`:CocInstall hoge`を実行する．`hoge`にはインストールしたいExtentionの名前を入れる．
Extentionの一覧は[`coc.nvim`のGithub](https://github.com/neoclide/coc.nvim#example-vim-configuration)に記載されている．

## 設定ファイルの生成

neovimを起動し，`:CocConfig`を実行する．設定ファイルの名前は`coc-settings.json`で，場所は特に設定していない場合，`~/.config/nvim/coc-settings.json`になっている．
ここに様々な設定を記載していく.Extentionに無いLanguage Serverを利用したい場合にも，この設定ファイルに記載することで利用可能になる．

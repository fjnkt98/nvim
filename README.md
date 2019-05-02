# NeoVimの環境構築のためのリポジトリ

2019/05/02，VimからNeoVimへの移行を決意した．理由は，Vimの環境構築の管理をGithubでしようとして挫折したから．NeoVimだと管理が比較的簡単そうなので移行することにした．

## 想定する環境

WindowsだとVSCodeを使うので，主にLinux上で使うことになる．Macは知らない．

自分が使うLinuxは主に

- Ubuntu 16.04LTS or 18.04LTS
- Raspbian 

の2つ．RaspbianはDebian系LinuxでありUbuntuとは少し勝手が違うので，NeoVimのインスト―ル時に手順が少し異なる．

## 参考にしたサイト

- [NeoVImのGithubリポジトリ](https://github.com/neovim/neovim/wiki/Installing-Neovim)
- [dein.vimのGithubリポジトリ](https://github.com/Shougo/dein.vim/blob/master/README.md)
- [Neovim の設定を綺麗に整理してみた](https://qiita.com/tamago3keran/items/cdfd66b627b3686846d2)：NeoVimの導入の際に参考にした
- [dein.vimによるプラグイン管理のマイベストプラクティス](https://qiita.com/kawaz/items/ee725f6214f91337b42b)：Githubによる管理を行う際に参考にした
- [Raspbianにneovimをインストールする](http://hiratarosu.hateblo.jp/entry/2019/01/26/205929)
- [RaspbianにUbuntu用のPPAを追加する](https://qiita.com/hnw/items/734f82bee26a40269c1b#fn1)
- [What Debian version are the different Ubuntu versions based on?](https://askubuntu.com/questions/445487/what-debian-version-are-the-different-ubuntu-versions-based-on)：Debian系のバージョンに対応するUbuntuのバージョンの表
- [raspbian（Raspberry Pi）のバージョン確認方法](https://www.checksite.jp/raspbian-version-check/)

## 作業手順

大まかな手順としては，

1. `NeoVim`本体のインストール
1. `dein.vim`のインストール 
1. リポジトリのクローン

となる．

### 1. NeoVim本体のインストール

#### Ubuntuの場合

UbuntuでNeoVimをインストールするには，`PPA`リポジトリを追加し，`apt`コマンドでインストールする．また，いくつかのPythonライブラリをインストールする必要がある．

```console
$sudo add-apt-repository ppa:neovim-ppa/stable
$sudo apt-get update
$sudo apt-get install neovim
$sudo apt-get install python-dev python-pip python3-dev python3-pip
```

2019/05/02現在，Ubuntu18.04LTSでは，`NeoVim 0.3.4`がインストールされた．

#### Raspbianの場合

NeoVimはDebianの公式リポジトリに対応してはいるが，それでインストール出来るバージョンはかなり古いバージョンしかない．なので，別の方法でインストールする．具体的には，RaspbianにUbuntu用のPPAリポジトリを追加し，そこからインストールする．

まず，Raspbianのバージョンを確認する．`lsb_release -a`コマンドで確認することが出来る．今回は`stretch`であったので，対応するUbuntuのバージョンは`xenial`であった．

次にUbuntu用のPPAリポジトリを登録する．
公開鍵の取得を行う．

```console
$sudo apt install dirmngr
$sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 9DBB0BE9366964F134855E2255F96FCF8231B6DD
```

リポジトリの情報を記述するファイルを生成する．

```console
$sudo touch /etc/apt/sources.list.d/neovim-ppa.list
```

ファイルの中には次の2行を記述する．

```none:/etc/apt/sources.list.d/neovim-ppa.list
deb http://ppa.launchpad.net/neovim-ppa/stable/ubuntu xenial main 
deb-src http://ppa.launchpad.net/neovim-ppa/stable/ubuntu xenial main
```

あとは`apt`コマンドでインストールすることが出来る．

```console
$sudo apt update
$sudo apt install neovim
```

インストールが完了したらバージョンを確認する．

```console
$nvim -v
```

### 2. dein.vimのインストール

プラグイン管理ツールである`dein.vim`をインストールしておく．一応，これが無い場合に自動インスト―ルするような設定は行っているが，念のため．
手順は[`dein.vim`のリポジトリ](https://github.com/Shougo/dein.vim/blob/master/README.md)に書いてある通りを実行すればよい．尚，設定ファイルは自分のリポジトリに既にあるため書かなくてよい．また，インストールするディレクトリは別段決まっているというわけではないが，ここでは`~/.cache/dein`とする．

```console
$curl https://raw.githubusercontent.com/Shougo/dein.vim/master/bin/installer.sh > installer.sh
$sh ./installer.sh ~/.cache/dein
```

### 3. リポジトリのクローン

管理用リポジトリをクローンする．クローンする場所は`~/.config`でなければならない．

```console
$cd ~/.config
$git clone git@github.com:Fkohta/nvim.git
```

あとはNeoVimを起動すれば，プラグインのインストール等を勝手にやってくれるはず．

---

## 依存するライブラリ等

### python-client

`pip`を使ってインストールする．

```console
$pip install pynvim
$pip3 install pynvim
```

### jedi

`deoplete-python`に必要．`pip`でインストール出来る．

```console
$pip install jedi
```

### clang

`deoplete-clang`に必要．`apt`でインストール出来る．

```console
$sudo apt install clang
```

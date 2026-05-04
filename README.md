# dotfiles

[chezmoi](https://chezmoi.io) で管理する個人 dotfiles。

## 最新化 (このマシンで設定を変える時)

```sh
chezmoi edit ~/.zshrc          # ソーステンプレートをエディタで開く
chezmoi diff                   # ~/ 配下への適用前に差分確認
chezmoi apply                  # ~/ 配下に反映
chezmoi cd                     # ソースリポジトリへ移動
# あとは git add / commit / push
```

別マシンで変えた変更を取り込む:

```sh
chezmoi update                 # git pull + chezmoi apply
```

## 新マシン初期構築

> 前提: mise が入っていること (`brew install mise` など、入れ方は問わない)

```sh
# 1. mise 経由で chezmoi と ghq を入れる
mise use -g chezmoi@latest ghq@latest

# 2. このリポジトリを ghq 配下に取得
ghq get -p git@github.com:kengo-k/dotfiles.git

# 3. chezmoi のソースを ghq の場所に向ける
mkdir -p ~/.config/chezmoi
echo "sourceDir = \"$HOME/ghq/github.com/kengo-k/dotfiles\"" > ~/.config/chezmoi/chezmoi.toml

# 4. ホームに反映
chezmoi apply

# 5. mise 管理の CLI ツール群を install
mise install
```

その後、必要に応じて手動セットアップ:

- 必要に応じてフォントをインストール (例: `HackGen`)
- ghostty / cmux / OrbStack / JetBrains Toolbox 等の各種アプリ

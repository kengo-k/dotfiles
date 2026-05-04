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
# 1. mise 経由で chezmoi を入れる
mise use -g chezmoi@latest

# 2. リポジトリを取得して反映 (~/.local/share/chezmoi に clone される)
chezmoi init --apply kengo-k

# 3. mise 管理の CLI ツール群を install
mise install
```

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

> 前提: mise が入っていること（例: macOS なら `brew install mise`）
>
> メモ: `kengo-k` は自分の GitHub アカウント（`chezmoi init` の引数省略形）。

```sh
# 1. mise 経由で chezmoi を入れる
mise use -g chezmoi@latest

# 2. リポジトリを取得 (~/.local/share/chezmoi に clone される)
chezmoi init kengo-k

# 3. 差分確認して反映（初回は特に必ず確認）
chezmoi diff                   # ソース(テンプレ)と ~/ 配下(生成物)の差分。生成物の直編集も、テンプレ更新で「次に入る変更」も確認できる
chezmoi apply                  # ~/ 配下へ反映

# 4. mise 管理の CLI ツール群を install
mise install
```

### よくある詰まりポイント

- **初回の安全確認**: いきなり反映する場合は `chezmoi init --apply kengo-k` でも良いが、上書きが不安なら `chezmoi diff` を挟む。
- **SSH/HTTPS**: 環境によっては SSH が通らないことがある。その場合は `chezmoi init https://github.com/kengo-k/dotfiles.git` のように URL 指定で回避。
- **`mise install` 失敗**: `mise doctor` / `mise install -v` で原因を当たる（ネットワーク/権限/ビルド依存が多い）。


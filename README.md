# dotfiles

[chezmoi](https://chezmoi.io) で管理する個人 dotfiles。

## 最新化 (このマシンで設定を変える時)

```sh
chezmoi edit ~/.zshrc          # ソーステンプレートをエディタで開く
chezmoi diff                   # ~/ 配下への適用前に差分確認
chezmoi apply                  # ~/ 配下に反映
```

別マシンで変えた変更を取り込む:

```sh
chezmoi update                 # git pull + chezmoi apply
```

## 新マシン初期構築

### セットアップ（初回のみ）

まず Homebrew を入れる（公式手順）:

- `https://brew.sh/`

Homebrew インストール後、Brewfile で brew 管理のツールを揃える:

```sh
brew bundle --file ./Brewfile
```

`chezmoi` がまだ無ければ入れる（例）:

```sh
brew install chezmoi
```

### 作業手順

```sh
# 1. リポジトリを取得
# `chezmoi init <github-account>` を実行すると、`<github-account>/dotfiles.git` が `~/.local/share/chezmoi` に clone される
chezmoi init kengo-k

# 2. 差分確認して反映（初回は特に必ず確認）
chezmoi diff                   # ソース(テンプレ)と ~/ 配下(生成物)の差分。生成物の直編集も、テンプレ更新で「次に入る変更」も確認できる
chezmoi apply                  # ~/ 配下へ反映

# 3. mise 管理の CLI ツール群を install（mise が無ければ先に入れる）
# brew install mise
mise install
```

### トラブルシューティング

- **`mise install` 失敗**: `mise doctor` / `mise install -v` で原因を当たる（ネットワーク/権限/ビルド依存が多い）。


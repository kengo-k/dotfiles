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

このリポジトリ自体（ghq 管理下の clone）を chezmoi のソースとして使う。
chezmoi 既定の `~/.local/share/chezmoi` は使わず、`sourceDir` でこの clone を指す。

### 1. Homebrew

公式手順で導入: `https://brew.sh/`

### 2. リポジトリを取得

ghq はこの後 mise で入るため、初回は git clone で ghq レイアウトに直接置く:

```sh
git clone https://github.com/kengo-k/dotfiles.git ~/ghq/github.com/kengo-k/dotfiles
```

### 3. brew 管理ツールを揃える（chezmoi もここで入る）

```sh
brew bundle --file ~/ghq/github.com/kengo-k/dotfiles/Brewfile
```

### 4. chezmoi にこの clone をソースとして指定

```sh
mkdir -p ~/.config/chezmoi
printf 'sourceDir = "%s/ghq/github.com/kengo-k/dotfiles"\n' "$HOME" \
  > ~/.config/chezmoi/chezmoi.toml
chezmoi source-path            # この clone のパスが表示されれば OK
```

### 5. 差分確認して反映

```sh
chezmoi diff                   # ソースと ~/ 配下の差分（初回は特に必ず確認）
chezmoi apply                  # ~/ 配下へ反映
```

### 6. mise 管理の CLI ツールを install

mise が無ければ先に入れる（公式インストーラ）。その後:

```sh
mise install
```

### トラブルシューティング

- **`mise install` 失敗**: `mise doctor` / `mise install -v` で原因を当たる（ネットワーク/権限/ビルド依存が多い）。
- **GitHub API rate limit (403)**: `github:` バックエンドの install 時に出ることがある。`gh` 認証済みなら `GITHUB_TOKEN="$(gh auth token)" mise install` で回避できる。


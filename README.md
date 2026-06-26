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

> **Linux で使う場合は zsh を別途インストールすること。**
> このリポジトリの設定は zsh 前提だが、macOS と違い Linux には zsh が同梱されない。
> ディストリ標準のインストール方法で入れる（例: Debian/Ubuntu は `sudo apt install zsh`）。
> 導入後、`chsh -s /usr/bin/zsh` でログインシェルを zsh に切り替える。

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

mise 本体はステップ3の `brew bundle` で導入済み。各ツールを入れる
（グローバル設定 `~/.config/mise/config.toml` を常に読むので、実行場所は任意）:

```sh
mise install
```

> **新マシンでは `GITHUB_TOKEN` がほぼ必須。**
> ツールの多くは `github:` バックエンドで、バージョン固定でも初回は GitHub API
> を叩く。未認証は 60 回/時で、ツール数分の問い合わせですぐ 403 になる。
> スコープ不要の PAT を `https://github.com/settings/tokens` で作り、渡して実行する。
> `read -s` で入力すれば、トークンが画面にもシェル履歴にも残らない:
>
> ```sh
> read -rs GITHUB_TOKEN && export GITHUB_TOKEN   # プロンプトにトークンを貼り付け（非表示）
> mise install
> ```
>
> 使い終わったら `unset GITHUB_TOKEN`、またはそのシェルを閉じれば残らない。
> `gh` 認証済みのマシンなら `GITHUB_TOKEN="$(gh auth token)" mise install` でもよいが、
> 初回ブートストラップ時点では `gh` 自体が未導入（mise で入れる対象）なので使えない。

### トラブルシューティング

- **`mise install` 失敗**: `mise doctor` / `mise install -v` で原因を当たる（ネットワーク/権限/ビルド依存が多い）。
- **GitHub API rate limit (403)**: 上記ステップ6の `GITHUB_TOKEN` を設定する。


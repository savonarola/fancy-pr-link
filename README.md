# GitHub PR Link generator

Being a GitHub user, I often need share my work: paste a link to a PR into Slack or other chat.

This tool helps to generate a link and decorate it a bit.

* The link will be copied to clipboard.
* When being pasted into Slack, it will be pasted as a rich text.
* The link will be decorated with a nice icon guessed from PR title.

## Requirements

- Mac OS
- [ollama](https://ollama.ai/)
- [fzf](https://github.com/junegunn/fzf)
- [gh](https://github.com/cli/cli)

## Installation

### ollama

Install ollama.
```bash
brew install ollama
```

Install some model.
```bash
# Or any other model you want
ollama pull llama3.2:3b
```

Run ollama server.
```bash
cp com.ollama.server.plist ~/Library/LaunchAgents/
launchctl load ~/Library/LaunchAgents/com.ollama.server.plist
```

### fzf and gh

```bash
brew install gh fzf
```

Login to GitHub
```bash
gh auth login
```

### `pbcopy-pr-link`

Copy [`pbcopy-pr-link`](./pbcopy-pr-link) script to a folder in your `$PATH`. Its a python script using only Foundation framework and standard python libraries, so it should work from everywhere without any env setup.

## Usage

```bash
id=$(GH_REPO=cli/cli gh pr list | fzf | cut -d $'\t' -f 1)
pbcopy-pr-link --pr-id "$id" --repo "cli/cli" --ollama-host "127.0.0.1:11534" --ollama-model "llama3.2:3b"
```

For convenience, you can add something like this to your shell config (`.zshrc`, `.bashrc`, etc.):

```bash
pr-link() {
    repo="$1"
    local GH_REPO="$repo"
    export GH_REPO
    id=$(gh pr list -A "@me" | fzf | cut -d $'\t' -f 1)
    if [[ -n $id ]]; then
        pbcopy-pr-link --pr-id "$id" --repo "$repo" --ollama-host "127.0.0.1:11534" --ollama-model "llama3.2:3b"
    fi
}
```

Use it like this:

```bash
pr-link OWNER/REPO
```

## Disclaimer

This project was made for fun, and I'm not sure if it will be useful for anyone else.
It is a bit more useful than https://github.com/savonarola/git-clown, though.

## License

[MIT](LICENSE)




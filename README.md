# Example Workflow

```yaml
name: test
on:
  pull_request:
    branches:
      - main
  push:
    branches:
      - main
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: jdx/mise-action@v1
        with:
          version: 2023.12.0 # [default: latest] mise version to install
          install: true # [default: true] run `mise install`
          cache: true # [default: true] cache mise using GitHub's cache
          # automatically write this .tool-versions file
          experimental: true # [default: false] enable experimental features
          tool_versions: |
            shellcheck 0.9.0
          # or, if you prefer .mise.toml format:
          mise_toml: |
            [tools]
            shellcheck = "0.9.0"
      - run: shellcheck scripts/*.sh
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: jdx/mise-action@v1
      # .tool-versions will be read from repo root
      - run: node ./my_app.js
```

Alternatively, mise is easy to use in GitHub Actions even without this:

```yaml
jobs:
  build:
    steps:
    - run: |
        curl https://mise.jdx.dev/install.sh | sh
        echo "$HOME/.local/share/mise/bin" >> $GITHUB_PATH
        echo "$HOME/.local/share/mise/shims" >> $GITHUB_PATH
```

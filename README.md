# Semantic Release

This project uses [semantic-release](https://github.com/semantic-release/semantic-release) for automated version management and package publishing.

## Setup

1. Install Semantic Release Git and Changelog plugins:

```bash
npm install --save-dev @semantic-release/git @semantic-release/changelog
```

2. Add the following to your `package.json`:

```json
  "private": true,
  "publishConfig": {
    "access": "public"
  },
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    "@semantic-release/github",
    "@semantic-release/npm",
    "@semantic-release/git"
  ],
  "release": {
    "prepare": [
      "@semantic-release/changelog",
      "@semantic-release/npm",
      {
        "path": "@semantic-release/git",
        "assets": [
          "package.json",
          "package-lock.json",
          "CHANGELOG.md"
        ],
        "message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}"
      }
    ]
  }
```

3. Setup GitHub Actions to run semantic-release:

```yaml
# .github/workflows/main.yml
```

```yaml
name: Semantic release

on:
  push:
    branches:
      - main

jobs:
  publish:
    runs-on: ubuntu-22.04

    permissions:
      contents: write
      issues: write
      pull-requests: write

    steps:
      - uses: actions/checkout@v2

      - name: Setup Node.js

        uses: actions/setup-node@v1

        with:
          node-version: 20

      - name: Install dependencies

        run: npm install

      - name: Build app

        run: npm run build

      - name: Semantic release

        env:
          GITHUB_TOKEN: ${{ secrets.GH_TOKEN }}

        run: npx semantic-release
```

## Usage

To release a new version, push a commit to the `main` branch.

```bash
git add .

git commit -m "feat: Add Semantic Release and GithubActions"

git push
```

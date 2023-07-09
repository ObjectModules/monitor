```
# Sample workflow for building and deploying a Jekyll site to GitHub Pages
name: Build and deploy Jekyll site

# 💎 Runs on deployment targeting the default branch
on:
  workflow_run:
    workflows: ["pages-build-deployment"]
    types: [completed] #requested

# 🪂 Allow one concurrent deployment
concurrency:
  group: "pages"
  cancel-in-progress: true

# Sets permissions of the GITHUB_TOKEN
permissions: write-all

jobs:
  # Build job
  github-pages:
    if: ${{ github.event.workflow_run.conclusion == 'success' }}
    runs-on: ubuntu-latest
    steps:
      - name: 📂 Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive

      - name: 🚀 Setup python
        uses: actions/setup-python@v4
        with:
          python-version: 3.8

      - name: 🔨 Get Linux-cache
        uses: actions/cache@v3
        with:
          path: vendor/bundle
          key: ${{ runner.os }}-cache--${{ hashFiles('**/Makefile') }}

      - name: 💎 Build and deploy
        uses: eq19/intro@v1
        with:
          bundler_ver: '2.3.7'
          pre_build_commands: 'make build'
          repository: ${{ secrets.REPOSITORY }}
          token: ${{ secrets.JEKYLL_GITHUB_TOKEN }}

```

[![](https://user-images.githubusercontent.com/8466209/207416875-064d5b66-e577-4dde-a329-123f9325d974.png)](https://gist.github.com/eq19/b9f901cda16e8a11dd24ee6b677ca288#file-locate-md)

[![2022-12-26 (1)](https://user-images.githubusercontent.com/8466209/209503644-968f7e29-ab14-4eff-abad-6a2bccd8398e.png)
](https://github.com/FeedMapping/feedmapping.github.io)

[![2022-12-26 (2)](https://user-images.githubusercontent.com/8466209/209503851-493bebbb-28e9-495b-87e5-8d9c17e24f0f.png)
](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow)


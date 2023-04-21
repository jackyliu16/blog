+++
title = "publish guideline of zola In github page"
date = 2023-04-21

[taxonomies]
tags = ["zola", "guideline"]
+++

> At the begin of the papaer, i have to suggest everyone that, you shouldn't write a long issue in github, you should write it down in your computer editor and then just paste it in github, which i have a sad review that i have finish this paper but due to the network connection issue, i lost it.

In this article, i will introduce another version of guideline of publish your zola website in github-page(gh-page).

there is two ways to publish the page, to generate and publish in one repository(A), or generate in repo A(private) and publish in repo B(public). I will splitly introduce how to configuration it both.

Firstly, i will introduce the commonness of this method. But at the begin, you should have create you repo, and one of your repo should be call as `<username>.github.io`.

1. you should follow the [guideline](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository#managing-github-actions-permissions-for-your-repository) to mark sure your actions permission is been setting to `Allow all actions ...`, and then Drag the page to buttom, [setting the workflow permission](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository#configuring-the-default-github_token-permissions) as `read and write permission` in repo A, which is been called as `GITHUB_TOKEN`.
*(there is no evidence suggest that repo B should do it both, but you can do it)*.
2. create `.github/workflows/<what every you want>.yml` and copy bellow into it. Don't forget to complete the `<>` before your commit the file.

```yaml
# On every push this script is executed
on: push
name: Build and deploy GH Pages
jobs:
  build:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/<your branch name>'
    steps:
      - name: checkout
        uses: actions/checkout@v3.0.0
      - name: build_and_deploy
        uses: shalzz/zola-deploy-action@v0.17.2
        env:
            # Target branch
            PAGES_BRANCH: gh-pages
            <TOKEN>
```

And then, here is the **different part** of this two method:

1. for single repo user, you should set `TOKEN` as `#TOKEN: ${{ secrets.GITHUB_TOKEN }}`
2. for AB repo user
    1. you should set `TOKEN` as `TOKEN: ${{ secrets.PUBLIC_TOKEN }}`
    2. you should get the [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#personal-access-tokens-classic) and [set it as a repo secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository).

At the end, please make sure all the `<>` has been fill. You should start to push your blog to check if the github page could successful deloy it.

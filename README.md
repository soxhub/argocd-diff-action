# ArgoCD Diff Github Action
This action generates a diff between the current PR and the current state of the cluster. 

Note that this includes any changes between your branch and latest master, as well as ways in which the cluster is out of sync. 

# How to use it

Example GH action:
```
name: ArgoCD Diff

on:
  pull_request:
    branches: [ master ]

jobs:
  argocd-diff:
    name: Generate ArgoCD Diff
    runs-on: ubuntu-latest
    steps:

      - name: Checkout repo
        uses: actions/checkout@v2

      - uses: quizlet/argocd-diff-action@master
        name: ArgoCD Diff
        with:
          argocd-server-url: argocd.example.com
          argocd-token: ${{ secrets.ARGOCD_TOKEN }}
          github-token: ${{ secrets.GITHUB_TOKEN }}
          argocd-version: v1.6.1
```

# How it works
1) Downloads the specified version of the ArgoCD binary, and makes it executable
2) Connects to the ArgoCD api using the argocd token, and gets all the apps
3) Filters the apps to the ones that live in the current repo
3) Runs `argocd app diff` for each app
5) Posts the diff output as a comment on the PR

# Publishing
Build the script and commit to your branch:
`npm run build && npm run pack`
Commit the build output, and make a PR


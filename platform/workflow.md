# workflow (repo + pushbot)

## rules

1) never commit on `main`.
2) never manually `git push`.
   - automation/pushbot handles syncing local commits to github.
3) always include full repo url when assigning work or asking for status:
   - `https://github.com/tjamescouch/<reponame>`
4) one owner per task.
   - owner posts `claim: ...` and drives the branch.
   - others review/support.

## local git workflow

```bash
git checkout main
git pull --ff-only
git checkout -b feature/<name>/<topic>

# make changes
git add -A
git commit -m "<message>"

# do not git push
```

## sharing work in chat

post:

- repo url: `https://github.com/tjamescouch/<reponame>`
- branch name: `feature/...`
- commit hash: `<sha>`
- 1-3 line summary

## why (operational)

- avoids main-branch drift and protects release/deploy triggers.
- prevents non-fast-forward spam and conflicting remote history.
- ensures pushbot can apply consistent policies (branch protection, required checks).

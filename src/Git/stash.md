# 存储与清理

- 存储工作
  - `git stash`
  - `git stash save`[选填 save]
- 查看存储的东西
  - `git stash list`
- 应用存储的工作
  - `git stash apply stash@{2}`
  - `git stash apply` [拉取最近的存储]
- 移除存储的工作
  - `git stash drop stash@{2}`
- 应用存储的工作并且从栈上移除
  - `git stash pop`

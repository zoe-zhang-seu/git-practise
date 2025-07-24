`git branch -r`

HEAD 实际上就是我们在哪一个branch, 指向哪一个commit


```
git remote show origin
```


## Git `fetch` + `merge` 流程总结

1. **分支概念**
   - `origin/master`：本地对远程 `master` 分支的“快照”（远程跟踪分支），由 `git fetch` 更新  
   - `master`：你当前检出的本地分支，实际控制你工作目录的内容  

2. **步骤详解**
   1. **拉取远程更新**  
      ```bash
      git fetch origin
      ```
      - 仅更新本地 `origin/master`，不修改当前 `master` 分支  
      - 工作区和本地分支保持不变  
   2. **合并远程改动**  
      ```bash
      git merge origin/master
      ```
      - 将 `origin/master` 的最新提交合并到本地 `master`  
      - 如果本地 `master` 没有额外提交，则会做 **fast‑forward**，直接把 `master` 移动到最新提交  
      - 如果本地有新提交，则会生成一个合并提交，将两条历史合并  

3. **合并示意图**  
   - **仅有快进**  
     ```
     o — A — B   ← origin/master
             \
              (无本地新提交)
     ⇒ o — A — B   ← master, origin/master
     ```
   - **有本地提交**  
     ```
         M   ← master (merge commit)
        / \
       C   B   ← origin/master
      /
     A   ← master 原位置
     ```

4. **快捷命令**
   - `git pull`  
     - 等价于 `git fetch` + `git merge`（默认模式）  
     - 可通过配置 `git config pull.rebase true` 将合并改为变基  

---

**核心要点**：  
- `git fetch` 只“拿”，不动本地分支；  
- `git merge origin/master` 才会把远程更新真正“合”进来；  
- `git pull` 是这两个命令的一步到位。 
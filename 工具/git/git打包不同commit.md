git archive -o latest.zip NEW_COMMIT_ID_HERE $(git diff --name-only OLD_COMMIT_ID_HERE NEW_COMMIT_ID_HERE)

(需要在git对应仓库的顶级目录执行)

def lastVersion = '3.0.0.2.2-rc_for_GA_20210719'

```
task diff(type: Exec){
    commandLine 'sh', 'diff.sh', lastVersion
}
```


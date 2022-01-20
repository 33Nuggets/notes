Goal: to rollback remote git to certain commit
from [here](https://stackoverflow.com/questions/5816688/resetting-remote-to-a-certain-commit)

```bash
git reset --hard <commit-hash>
git push -f origin master
```
However, if the intermidiate commits(between latest and the target rollback commit) are important, you shouldn't do it because you cannot undo.
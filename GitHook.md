
This pre-commit hook allow to not commit file with only change on line 

```swift
//  Created by <author> on <date>
```

because this change is not relevant, and make commit to big for nothing after regenaring an app

```bash
#!/bin/sh

# Redirect output to stderr.
exec 1>&2

############################################
# Do not commit file with only date modifyed
############################################

# Get file with one line updated
files=$(git diff-files --stat | grep "2 +-" | sed "s/ | 2 +-//g")
for file in $files
do
  createdModify=$(git diff-files --patch  $file | grep "+//  Created by" | wc -l)
  if [ $createdModify -eq 1 ] # filter on one wher only Creat by line is edited
  then
 	 echo $file
	 git checkout HEAD -- "$file" # reset this file, no need to update date only
  fi
done
```

## where to put?

in `.git/hook/pre-commit` file. must be executable (chmod +x)

or in a custom folder like `.githook`(maybe already there)
and configure with command 
```
git config core.hooksPath .githooks
```

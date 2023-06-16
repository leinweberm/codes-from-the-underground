# **GIT**

## F&Q

> -   **Jak zresetovat mou lokální větev pokud je detached případně pokud je o commit popředu?**

```yaml
git fetch
git reset --hard origin/<jméno_větve>
```

> -   **Omylem jsem commitnul změny do jiné větve než jsem chtěl, co teď?** <br/>

```yaml
# Změny jsem commitnul ale ještě nepushnul do remote originu:
	Cherry pick commit using GUI of your git host like GitHub, GitLab, etc...

# I haven't pushed changes into remote origin yet:

	# v1 - checkout to desired branch and merge
	git checkout <branch_you_wanted_to_commit_to>
	git merge <branch_you_accidentally_commited_to>

	# v2 - creat new branch from current branch and push it to remote
	git branch <new_branch_name>
	git checkout <new_branch_name>
	git push -u origin <new_branch_name>
```

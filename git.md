# GIT

## Commands
### To clear entire cache and staging area:
`git rm -r cached`

### Revert any changes
`git reset --hard`

### Hard reset specific branch
`reset repoName/branchName --hard`

## Problems
> fatal: unable to access 'https://github.com/xxxx.git/': SSL certificate problem: self signed certificate in certificate chain
`git config --global http.sslVerify true`
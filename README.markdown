##necessary instructions:
new post: `rake new_post["title"]`

###Deploying to Github Pages:

```shell
rake setup_github_pages
rake generate
rake deploy
git add .
git commit -m 'your message'
git push origin source
rake setup_github_pages
rake generate
rake deploy
git remote add origin (your repo url)
# set your new origin as the default branch
git config branch.master.remote origin
echo 'your-domain.com' >> source/CNAME
```
[deploying to github](http://octopress.org/docs/deploying/github/)

add github as remote: `git remote add github https://github.com/mizanrahman/octopress.git`

### markdown cheatsheet

[link](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

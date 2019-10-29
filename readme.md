### Create a new repository on the command line

```markdown
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/GitVide/test.git
git push -u origin master
```



### Or push an existing repository from the command line

```markdown
git remote add origin https://github.com/GitVide/test.git
git push -u origin master
```
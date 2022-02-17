# GitHub at SF
Not much here yet, but feel free to add a new section with Git related information below.

## Working with Github templates and branches with specific functionality:
Since we have (Satus)[https://github.com/studio-freight/satus], we should always try to use it as a base for our projects, to keep it simple we've created branches with specific functionality, i.e. `with-shopify`, `with-contentful`. Here's a simple guide on how to use those branches and update them with the latest goodies from Satus

1. Go to the template and click 'use this template' button.
2. Create a new project and check 'Include all branches' option.
3. Turn the branch on the new project you want to use into `main`. You can do it from Github branches or rebasing/merging.
4. Add template as remote using `git remote add template [repository URL]`

To pull changes from template branch "example":
1. `git checkout -b example`
2. `git pull template example`
3. `git switch main`
4. `git rebase example`
5. `git push --force`

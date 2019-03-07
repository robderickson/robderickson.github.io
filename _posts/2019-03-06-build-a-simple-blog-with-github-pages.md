---
layout: post
title: "Build a Simple Blog with Github Pages"
author: rob_derickson
tags: githubpages,github,blog,jekyll
---

I struggled a little setting this blog up, so I figured I would blog about it. I didn't want anyting too fancy, and I didn't want to touch anything Ruby-related to support Jekyll themes.

This is the process I used to create a minimal blog using only what is natively available from GitHub. All of the documentation I followed can be found under "Related Links" at the bottom of the post.

## Create your GitHub Pages repository and choose a Jekyll theme
1. Login to GitHub, and create a new repository with a README: https://github.com/new
2. Name your repository _`username`_`.github.io`.  
    **Note:** This _must_ match your GitHub username.
3. Clone your new repository: `git clone https://github.com/`_`username`_`/`_`username`_`.github.io`
4. Back on the GitHub site, click **Settings**.  
![Repository Settings button screenshot](/assets/githubpages-settings.png)  
5. Scroll down to the _GitHub Pages_ section and click **Choose Theme**.  
![Choose Theme button screenshot](/assets/githubpages-choosetheme.png)  
6. Select a theme (I liked Minimal) and click **Select Theme**.
7. Click **Commit changes** to commit the GitHub Pages sample to your `master` branch.
8. Browse to `https://`_`username`_`.github.io` to see your new blog!
9. Now pull all of your hard work to your local repository: `git pull origin master`

## Create content your new blog
In my local repository, I have an `author` branch where I plan to commit content I am not ready to publish:
```
git branch author
git checkout author
```  

To publish blog posts, you will need to create a folder named `_posts`, where you will save specially-formatted Markdown files.

1. Create a directory named `_posts` in your local repository.
2. Create a new Markdown file named using the following format: _`YYYY`_`-`_`MM`_`-`_`DD`_`-`_`title`_`.md`  
    **Example:** `2019-03-06-build-a-simple-blog-with-github-pages-and-jekyll.md`
3. Your Markdown file should contain metadata (aka "front matter") to declare the layout of your post and set the browser's title bar.  
    ```markdown
    ---
    layout: post
    title: "My first post!"
    ---

    # My first post!
    Welcome to my blog. This is my first post!
    ```
4. Save your Markdown file, "add" your `_posts` directory to your repo, and then stage and commit your work:  
    ```
    git add _posts
    git commit -a -m "My first post!"
    ```

## Publish your content
If you were to push your local repo back to GitHub at this point, you would still only see whatever is in `README.md`. It is pobably the template sample page if you committed it after selecting a template. You will need to modify your `README.md` to show off your new content.

1. Open your `README.md` file, and replace its contents with the following markup:  
    ```markdown
    
    { "{% for post in site.posts " }}%}
    **[{{ "{{ post.title " }}}}]({{ "{{ post.url " }}}})**  
    {{ '{{ post.date | date: "%Y-%m-%d" ' }}}}  
    {{ "{{ post.excerpt " }}}}  
    {{ "{% endfor " }}%}
    ```
2. If you created an `author` branch like I did, merge it into your local `master`, and push your `master` branch back to GitHub:  
    ```
    git checkout master
    git merge author
    git push origin master
    ```

Your first post should now be visible on your new blog. You can also create additional pages to your blog, and do some more magic with front matter.

## Related links
* [Create a GitHub Pages site](https://pages.github.com/)
* [Add a Jekyll theme to Github Pages](https://help.github.com/en/articles/adding-a-jekyll-theme-to-your-github-pages-site-with-the-jekyll-theme-chooser)
* [Create posts](https://jekyllrb.com/docs/posts/)
* [Create pages](https://jekyllrb.com/docs/pages/)
* [Read more about front matter](https://jekyllrb.com/docs/front-matter/)
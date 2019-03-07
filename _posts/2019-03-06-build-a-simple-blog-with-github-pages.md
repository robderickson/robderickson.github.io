# Build a Simple Blog with Github Pages
I struggled a little setting this blog up, so I figured I would blog about it. I didn't want anyting too fancy, and I didn't want to touch anything Ruby-related to support Jekyll themes.

This is the process I used to create a minimal blog using only what is natively available from GitHub. All of the documentation I followed can be found under "Related Links" at the bottom of the post.

## Create your GitHub Pages repository and choose a Jekyll theme
1. Login to GitHub, and create a new repository with a README: https://github.com/new
2. Name your repository _`username`_`.github.io`.  
    **Note:** This _must_ match your GitHub username.
3. Clone your new repository: `git clone https://github.com/`_`username`_`/`_`username`_`.github.io`
4. Back on the GitHub site, click **Settings**.  
![Repository Settings button screenshot](../assets/github-settings.png)  
5. Scroll down to the _GitHub Pages_ section and click **Choose Theme**.  
![Choose Theme button screenshot](../assets/github-choostheme.png)  
6. Select a theme (I liked Minimal) and click **Select Theme**.
7. Click **Commit changes** to commit the GitHub Pages sample to your `master` branch.
8. Browse to `https://`_`username`_`.github.io` to see your new blog!
9. Now pull all of your hard work to your local repository: `git pull origin master`

## Publish content to your new blog
In my local repository, I have an `author` branch where I plan to commit content I am not ready to publish:  

    ```
    git branch author
    git checkout author
    ```
#### Posts
To publish blog posts, you will need to create a folder name `_posts`, where you will save specially-formatted Markdown files.

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
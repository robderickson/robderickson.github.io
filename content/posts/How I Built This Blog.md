---
author: "Rob Derickson"
title: "How I Built This Blog"
date: 2026-03-07
---

## Create a GitHub repository

1. Create a new GitHub repository named `<user name>.github.io` where `<user name>` is your GitHub user name.
1. Clone the new repository.

## Install Hugo

NOTE: Since I built this on Fedora 64-bit, so I'll include instructions for that. See [Hugo's installation instructions](https://gohugo.io/installation/) for your operating system.

1. Download a Hugo release for your operating system from [Hugo's releases on GitHub](https://github.com/gohugoio/hugo/releases). I downloaded `hugo_<version>_linux-amd64.tar.gz`
1. Extract the Hugo binary:
    ```bash
    tar -xtvf ./hugo_<version>_linux-amd64.tar.gz
    ```
1. Move the binary to `/usr/local/bin`:
    ```bash
    mv ./hugo /usr/local/bin/
    ```
1. Make the binary executable:
    ```bash
    chmod +x /usr/local/bin/hugo
    ```

## Create a new Hugo site and install a theme

1. In the parent directory containing your `<user name>.github.io` repository, create a new site:
    ```bash
    hugo new site <username>.github.com --force
    ```
    NOTE: The `--force` option is required because a folder named `<user name>.github.io` already exists.
1. Check out [Hugo themes gallery](https://themes.gohugo.io) to select a theme you like. I'm using the Paper theme.
1. Set your location to your repository directory and add the chosen theme as a submodule in your repository.
    ```bash
    cd ./<user name>.github.io
    git submodule add https://github.com/halogenica/beautifulhugo.git themes/beautifulhugo
    ```
1. Update `hugo.toml` to include a key for your theme.
    ```bash
    echo "theme = 'beautifulhugo'" >> hugo.toml
    ```
1. Start the Hugo server and browse to `http://localhost:1313` to confirm it is working.
    ```bash
    hugo server
    ```

## Configure your site

Many configuration items will depend on your theme. See your theme's README for details. My `hugo.toml` looks something like this:

```toml
baseURL = 'https://robderickson.com/'
languageCode = 'en-us'
title = 'This is a... Blog?'
theme = 'beautifulhugo'

[Params]
    hometitle = 'This is a... Blog?'
    subtitle = 'by Rob Derickson'
    rss = true
    [Params.author]
        name = 'Rob Derickson'
        linkedin = 'robderickson'
        github = 'robderickson'
```

## Create your first post

1. Create a folder named `posts` in your repository's `content` folder.
    ```bash
    mkdir ./content/posts
    ```
1. Draft a post in Markdown, and ensure the post has front matter like the following:
   ```markdown
   ---
   author: "Rob Derickson"
   title: "How I Built This Blog"
   date: 2026-03-07
   ---
   ```
1. Save your post to `./content/posts` and browse to `http://localhost:1313` to test your site locally.

## Deploy your site with GitHub Actions

See [Host on GitHub Pages](http://gohugo.io/host-and-deploy/host-on-github-pages/) for step-by-step instructions on how to setup a GitHub Actions workflow to deploy your site.

In your `hugo.yaml` workflow document, customize the `jobs/build/env/TZ:` key to match your time zone. I set mine to `America/New_York` for the US Eastern time zone. For a list of valid IANA time zones, see Wikipedia's [List of tz database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

## (Optional) Configure a custom domain

1. Open your repository on github.com.
1. Click **Settings** and then click **Pages**.
1. Under **Custom domain** type your domain name and click **Save**.
1. In your DNS zone host (usually your domain registrar), create DNS A and AAAA records like the following:

    | Type | Name | Content |
    |------|------|---------|
    | A | @ | 185.199.108.153 |
    | A | @ | 185.199.109.153 |
    | A | @ | 185.199.110.153 |
    | A | @ | 185.199.111.153 |
    | AAAA | @ | 2606:50c0:8000::153 |
    | AAAA | @ | 2606:50c0:8001::153 |
    | AAAA | @ | 2606:50c0:8002::153 |
    | AAAA | @ | 2606:50c0:8003::153 |

1. Wait a couple of hours for DNS propagation to complete, and then try your custom domain to see if it works.

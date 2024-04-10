# [jurgott.github.io](https://jurgott.github.io/)

## Setup a new website (one time setup)

To start a new website using `hugo` with `hugo-geekdoc` theme,

```
hugo new site ./ --force
mkdir -p themes/hugo-geekdoc/
curl -L https://github.com/thegeeklab/hugo-geekdoc/releases/latest/download/hugo-geekdoc.tar.gz | tar -xz -C themes/hugo-geekdoc/ --strip-components=1
```

## Configure website (one time setup; can be updated later as needed)

Then edit `config.toml` file to include site and theme relevant configuration. An example can be found at: 

https://geekdocs.de/usage/configuration/

Here, I take that example site configuration and made several changes using a text editor:

1. Use URL in the `config.toml` file to reflect our website
2. Comment out `logo.png` file because we don't have it yet; default will be used
3. Change `geekdocMenuBundle` to `false` to use default plain side navigation

The theme used here was a template developed at https://github.com/thegeeklab/hugo-geekdoc. 
Although it is possible to customize this theme for our own needs, it may need trial and error with tedious work.
Therefore we should stick to the options that `config.toml` provides and customize it using those provided options without hacking further into the theme.

## Add, remove and edit webpages

All webpage contents are under `content` folder, written in Markdown format. 
It should be quite intuitive if you imagine this folder being the root folder of your website.
The `index.html` pages are `_index.md` this is reserved naming convention that you should not change. 
So imagine you create a new folder called `lectures` and create a document called `lectures/_index.md`, then this page will be available as: `jurgott.github.io/lectures`.

For regular webpages just use any regular file names. For example if you have a document called `lectures/linkage.md` then this page will be available as: `jurgott.github.io/lectures/linkage.html`.

You can add / remove / edit folders and files under `content`. To link to different pages you can either use absolute URL link, or use relative link, in markdown syntax. I have shown both approaches in [this file](https://raw.githubusercontent.com/jurgott/jurgott.github.io/main/content/_index.md) see the "Computer Programs" section there are both relative and absolute URL.

Here is a [starting point to learn Markdown syntax](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

## Preview site locally (recommended every time after you update your website)

Once you finish editing, you should preview your changes locally before you publish, to make sure your edits are correctly rendered as HTML. To do so, you need to first [install HUGO](https://gohugo.io/installation/). 
After you install it you should have `hugo` command available from your terminal. Then via command terminal you `cd` into the local clone of this repo on your PC (where `config.toml` file locates, which is the parent folder of `content`), and type:

```
hugo serve --disableFastRender
```

wait until it prints on the terminal something like this:

```
Web Server is available at //localhost:1313/ (bind address 127.0.0.1) 
Press Ctrl+C to stop
```

then you visit this page `localhost:1313` by typing this to your web browser, you should see the website locally. 

**If you keep this running on the background and make changes to your local files, then the website will render real-time to reflect the changes you made**. This is perhaps the best way to edit your website to make sure it works. If there is an error message that means you've done something wrong. You will have to revert your change and see where you made a mistake.

## Host site on GitHub (one-time setup)

To set it up: https://gohugo.io/hosting-and-deployment/hosting-on-github/

This is already done for this website. So after you are done with your changes, just push the updated repository to GitHub. You can track the progress of the build via this link:

https://github.com/jurgott/jurgott.github.io/deployments/github-pages

Or, simply wait for a few minutes. Your website should be updated online.


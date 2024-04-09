# [jurgott.github.io](https://jurgott.github.io/)

## Setup a new website

To start a new website using `hugo` with `hugo-geekdoc` theme,

```
hugo new site ./ --force
mkdir -p themes/hugo-geekdoc/
curl -L https://github.com/thegeeklab/hugo-geekdoc/releases/latest/download/hugo-geekdoc.tar.gz | tar -xz -C themes/hugo-geekdoc/ --strip-components=1
```

## Configure website

Then edit `config.toml` file to include site and theme relevant configuration. An example can be found at: 

https://geekdocs.de/usage/configuration/

Here, I take that example site configuration and made several changes:

1. Use URL in the `config.toml` file to reflect our website
2. Comment out `logo.png` file because we don't have it yet; default will be used
3. Change `geekdocMenuBundle` to `false` to use default plain side navigation

## Build site

Built and preview,

```
hugo serve
```

## Host site on GitHub

See here for details: https://gohugo.io/hosting-and-deployment/hosting-on-github/

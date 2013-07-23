# A Gollum Wiki for Stackato

[gollum](https://github.com/gollum/gollum) is a wiki backed by git.

## Deploying to Stackato

This gollum wiki can be deployed to any Stackato PaaS. 

Before pushing to Stackato, first run `bundle install`:

```bash
  $ bundle install
```

Note that the Gemfile pins [nokogiri](http://nokogiri.org/) to version 1.5.10 to avoid some build problems on Ubuntu 12.04 (the base image for Stackato 2.10).

Once `bundle install` completes, `stackato push` this to your targetted API endpoint:

```bash
  $ stackato push -n [optional-name]
```

The *stackato.yml* specifies the start command: `gollum --port $PORT`

## Using a git-based wiki with Stackato

Gollum uses git to store content. If you make edits on the *deployed* copy of the wiki, you might want to log in to your application container and push those changes to your own upstream repo. For example:

```bash
  $ stackato ssh gollum-wiki
```
First remove this upstream repo, you won't be able to push to it.
```bash
  gollum-wiki.stacka.to:~$ git remote rm origin
```
Next, add an upstream URL on a git server. I'd suggest using the HTTPS URL rather than messing with RSA keys in Stackato application containers.
```bash
  gollum-wiki.stacka.to:~$ git remote add origin https://github.com/you/gollum-wiki.git
  gollum-wiki.stacka.to:~$ git push
  Username for 'https://github.com': you
  Password for 'https://you@github.com': 
  ...
```

See also the [README](README) for gollum from the [source repository](https://github.com/gollum/gollum).
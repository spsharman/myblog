---
layout: post
title:  "Jekyll Installation"
author: Steve Sharman
date:   1970-01-31 00:00:00 +0000
published: true
categories: ACI
tags: ACI
comments: true
---

# Jekyll Installation

```
SSHARMAN-M-35ZJ:GitHub ssharman$ jekyll new blog
Running bundle install in /Users/ssharman/GitHub/blog...
  Bundler: The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.
  Bundler: Fetching gem metadata from https://rubygems.org/...........
  Bundler: Fetching gem metadata from https://rubygems.org/.
  Bundler: Resolving dependencies...
  Bundler: Using public_suffix 3.0.2
  Bundler: Using addressable 2.5.2
  Bundler: Using bundler 1.16.1
  Bundler: Using colorator 1.1.0
  Bundler: Using concurrent-ruby 1.0.5
  Bundler: Using eventmachine 1.2.5
  Bundler: Using http_parser.rb 0.6.0
  Bundler: Using em-websocket 0.5.1
  Bundler: Using ffi 1.9.23
  Bundler: Using forwardable-extended 2.6.0
  Bundler: Using i18n 0.9.5
  Bundler: Using rb-fsevent 0.10.3
  Bundler: Using rb-inotify 0.9.10
  Bundler: Using sass-listen 4.0.0
  Bundler: Using sass 3.5.6
  Bundler: Using jekyll-sass-converter 1.5.2
  Bundler: Using ruby_dep 1.5.0
  Bundler: Using listen 3.1.5
  Bundler: Using jekyll-watch 2.0.0
  Bundler: Using kramdown 1.16.2
  Bundler: Using liquid 4.0.0
  Bundler: Using mercenary 0.3.6
  Bundler: Using pathutil 0.16.1
  Bundler: Using rouge 3.1.1
  Bundler: Using safe_yaml 1.0.4
  Bundler: Using jekyll 3.7.3
  Bundler: Using jekyll-feed 0.9.3
  Bundler: Using jekyll-seo-tag 2.4.0
  Bundler: Fetching minima 2.4.1
  Bundler: Installing minima 2.4.1
  Bundler: Bundle complete! 4 Gemfile dependencies, 29 gems now installed.
  Bundler: Use `bundle info [gemname]` to see where a bundled gem is installed.
New jekyll site installed in /Users/ssharman/GitHub/blog.
SSHARMAN-M-35ZJ:GitHub ssharman$
```
## Change directory to /blog
`SSHARMAN-M-35ZJ:GitHub ssharman$ cd blog/`

## Start Jekyll locally
```
SSHARMAN-M-35ZJ:blog ssharman$ bundle exec jekyll serve
Configuration file: /Users/ssharman/GitHub/blog/_config.yml
            Source: /Users/ssharman/GitHub/blog
       Destination: /Users/ssharman/GitHub/blog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.649 seconds.
 Auto-regeneration: enabled for '/Users/ssharman/GitHub/blog'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

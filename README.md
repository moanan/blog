# Setup Hexo
```
$ brew install node`
$ npm install -g hexo-cli`
$ npm install hexo-deployer-git --save`
```
# Setup Theme Maupassant
```
$ cd /your_blog_dir/
$ git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant
$ npm install hexo-renderer-pug --save
$ npm install hexo-renderer-sass --save
```

# Usage
create new post
`$ hexo new 2020-01-01-whatever`

start local server
`$ hexo s`

generate static files
`$ hexo g`

deploy to github pages
`$ hexo d`

# Optional: use docker
[Docker hexo-cli](https://github.com/martindsouza/docker-hexo) doesn't work out of box with Theme Maupassant. You need to modify
```
FROM node:14.16.0-alpine3.10 as hexo-base
```
and add
```
  npm install hexo-renderer-pug --save && \
  npm install hexo-renderer-sass --save && \
```
and build your own docker image.

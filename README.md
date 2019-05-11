# Setup Hexo
`$ brew install node`

`$ npm install -g hexo-cli`

`$ npm install hexo-deployer-git --save`

# Setup Theme
\# change to workdir

`$ cd /your_blog_dir/`

\# install dependencies

`$ npm i -S hexo-generator-search hexo-generator-feed hexo-renderer-less hexo-autoprefixer hexo-generator-json-content`

\# download source

`$ git clone https://github.com/stkevintan/hexo-theme-material-flow themes/material-flow# New Document`

# Usage
create new post
`$ hexo new 2020-01-01-whatever`

start local server
`$ hexo s`

generate static files
`$ hexo g`

deploy to github pages
`$ hexo d`
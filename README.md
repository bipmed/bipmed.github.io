# BIPMed website source code

## Requirements

Install Ruby 2.7

```bash
brew install ruby@2.7
echo 'export PATH="/usr/local/opt/ruby/bin:/usr/local/lib/ruby/gems/2.7.0/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

Install bundle jekyll

```bash
gem install bundle jekyll
```

Install site dependencies

```bash
bundle install
```

Test website locally

```bash
bundle exec jekyll serve
```

Build statics files to deploy

```bash
jekyll build
```

The `_sites` folder contains website files. Change the content of this folder to the following folder.

```bash
bipmed.github.io
```

DO NOT delete the file CNAME

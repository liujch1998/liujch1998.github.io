# liujch1998.github.io

## New: Ruby 3.1.2 seems to support M1 (arm64)

### Setup

Follow [this tutorial](https://jekyllrb.com/docs/installation/macos/) to set up Ruby

```
gem install --user-install jekyll bundler
bundle config set --local path 'vendor/bundle'
bundle install
<!-- https://talk.jekyllrb.com/t/load-error-cannot-load-such-file-webrick/5417/6 -->
bundle add webrick
```

### Run

```
arch -x86_64 bundle exec jekyll serve
```

## Old: Workaround with Ruby 2.6 and Rosetta (x86_64)

### Setup

```
arch -x86_64 gem install --user-install jekyll bundler
arch -x86_64 bundle config set --local path 'vendor/bundle'
arch -x86_64 bundle install
```

### Run

```
arch -x86_64 bundle exec jekyll serve
```


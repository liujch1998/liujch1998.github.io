arch -x86_64 gem install --user-install jekyll bundler
arch -x86_64 bundle config set --local path 'vendor/bundle'
arch -x86_64 bundle install
arch -x86_64 bundle exec jekyll serve

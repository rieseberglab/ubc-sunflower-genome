## UBC Sunflower Genome Dataset

This is the repository holding the documentation for the UBC Sunflower Genome Dataset.

You can navigate the site on "https://rieseberglab.github.io/ubc-sunflower-genome/"

Whenever changes are committed to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages of the site, 
from the content in your Markdown files.


### Building the page from your dev environment

 1. Once: install RVM

        # based on https://rvm.io/rvm/install .
        gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
        curl -sSL https://get.rvm.io | bash -s stable --ruby

 1. Once: install Ruby (> 2.1.0)

        source ~/.rvm/scripts/rvm

        # this ruby version matches the one in .ruby-version
        rvm install ruby-2.4.5

        # set as default (optional)
        rvm --default use 2.4.5

 1. Once: install additional ruby gems in your custom ruby install

        cd <repodir> && rvm use
        gem install bundler
        bundle install # (installs stuff from ./Gemfile)

 1. Each time: When working on the site, use the same version of ruby with the project's Gemset

        source ~/.rvm/scripts/rvm
        cd <repodir> && rvm use

 1. The rest, building, deploying, is jekyll specific
    See https://help.github.com/en/articles/setting-up-your-github-pages-site-locally-with-jekyll

    View local site:

	    `bundle exec jekyll serve`

    (this makes the site available for you to browse at localhost:4000/, and will regenerate the site
     when files change)

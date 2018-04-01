# README

This is the Philly 2600 site, located at [philly2600.github.io](https://philly2600.github.io).

It is currently running [Jekyll](https://jekyllrb.com) with the [darcli](https://github.com/gjuniioor/darcli) theme.

## Build Your Own Local Copy

Make sure that you have a recent version of `ruby` installed as well as `ruby-dev`. Jekyll requires a `ruby` version of 2.2.5 or above at the time of writing.

```
$ sudo apt-get install -y ruby ruby-dev
$ ruby --version
ruby 2.3.3p222 (2016-11-21) [x86_64-linux-gnu]
```

Then, install `jekyll` and `bundler` using `gem`:

```
$ gem install jekyll bundler
```

Next, clone this repo and `cd` to it:

```
$ git clone https://github.com/philly2600/philly2600.github.io.git
$ cd philly2600.github.io
```

Finally, start `jekyll`:

```
$ bundle exec jekyll serve
```

Now you can point your browser to [http://localhost:4000](http://localhost:4000) to see the site.

## Submit Your Own Posts/Updates

Philly 2600 members are free to write for the site and make updates.

Blog posts are located in the `_posts/posts` folder and you can easily duplicate an existing post to fasion your own. Note, the filename must start with a date in **YYYY-MM-DD** format. Additionally, each post has an element in the front matter at the top called `author`. If you are a new author to the site, please create an author file in `_posts/team`, again naming the file in the same format with the date of your first article, and fill out the info inside mathing the post's `author` element to this file's `title` element.

Blog posts can be submitted to the site through standard GitHub [pull requests](https://github.com/philly2600/philly2600.github.io/pulls).

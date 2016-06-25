# rake js:routes

[![Build Status](https://travis-ci.org/yuku-t/js_rails_routes.svg?branch=master)](https://travis-ci.org/yuku-t/js_rails_routes) [![Code Climate](https://codeclimate.com/github/yuku-t/js_rails_routes/badges/gpa.svg)](https://codeclimate.com/github/yuku-t/js_rails_routes) [![Test Coverage](https://codeclimate.com/github/yuku-t/js_rails_routes/badges/coverage.svg)](https://codeclimate.com/github/yuku-t/js_rails_routes/coverage)

Generate a ES6 module that contains Rails routes.

## Description

This gem provides "js:routes" rake task.
It generates a ES6 requirable module with exports url helper functions defined by your Rails application.

Suppose a Rails app has some routes:

```rb
# == Route Map
#
#       Prefix Verb   URI Pattern                  Controller#Action
#     articles GET    /articles(.:format)          articles#index
#              POST   /articles(.:format)          articles#create
#  new_article GET    /articles/new(.:format)      articles#new
# edit_article GET    /articles/:id/edit(.:format) articles#edit
#      article GET    /articles/:id(.:format)      articles#show
#              PATCH  /articles/:id(.:format)      articles#update
#              PUT    /articles/:id(.:format)      articles#update
#              DELETE /articles/:id(.:format)      articles#destroy
Rails.application.routes.draw do
  resources :articles
end
```

This gem generates a JS file looks like this:

```js
// Don't edit manually. `rake js:routes` generates this file.
export function article_path(params) { return '/articles/' + params.id + ''; }
export function articles_path(params) { return '/articles'; }
export function edit_article_path(params) { return '/articles/' + params.id + '/edit'; }
export function new_article_path(params) { return '/articles/new'; }
```

## VS.

[railsware/js-routes](https://github.com/railsware/js-routes) spreads url helpers via global variable.

This gem uses ES6 modules.

## Requirement

- Rails >= 3.2

## Usage

Generate routes file.

```
rake js:routes
```

### Configuration

JSRailsRoutes supports several parameters:

Name       | Type     | Description                             | Default
-----------|----------|-----------------------------------------|----------------------------------------
`includes` | `Regexp` | routes match to the regexp are included | `/.*/`
`excludes` | `Regexp` | routes match to the regexp are excluded | `/^$/`
`path`     | `String` | JS file path                            | app/assets/javascripts/rails-routes.js

You can configure via `JSRailsRoutes.configure`.

```
# Rakefile
JSRailsRoutes.configure |c|
  c.excludes = %r{^/(rails|sidekiq)}
  c.path = 'path/to/rails-routes.js'
end
```

Now `rake js:routes` ignores paths starting with "/rails" or "/sidekiq".

### Command line parameters

You can override the coniguration via command line parameters:

```
rake js:routes excludes='^/rails'
```

The command still ignores "/rails" but includes "/sidekiq".

## Install

Your Rails Gemfile:

```
gem 'js_rails_routes', group: :development
```

## License

[MIT](https://github.com/yuku-t/js_rails_routes/blob/master/LICENCE)

## Author

[mizchi](https://github.com/mizchi) wrote "js:routes" task with referencing [mtrpcic/js-routes](https://github.com/mtrpcic/js-routes).

[yuku-t](https://yuku-t.com) refactor and improve the mizchi's script and published to rubygems.

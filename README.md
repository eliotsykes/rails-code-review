# Rails Code Review

An evolving set of guidelines & supporting reasons to consider when code reviewing Ruby on Rails apps.

<!-- MarkdownTOC depth=0 autolink=true bracket=round -->

- [Security](#security)
  - [Patch Rails Security Vulnerabilities](#patch-rails-security-vulnerabilities)
  - [Exposed Secrets](#exposed-secrets)
  - [Gem Versions](#gem-versions)
  - [Force SSL](#force-ssl)
- [Controllers](#controllers)
  - [Use HTTP Status Code Symbols](#use-http-status-code-symbols)
- [Presentation & Accessibility](#presentation--accessibility)
  - [Good Page Titles](#good-page-titles)
- [Database](#database)
  - [Healthy schema.rb](#healthy-schemarb)
- [Version Control](#version-control)
  - [Tidy Commit History](#tidy-commit-history)
- [Documentation](#documentation)
  - [Kickstart New Developers](#kickstart-new-developers)
- [Contributors](#contributors)

<!-- /MarkdownTOC -->


# Security

## Patch Rails Security Vulnerabilities

Update Rails to the latest patch version. Edit the rails version in the Gemfile then run `bundle install`. Ensure the version of Rails you're on is still actively supported and patched.

## Exposed Secrets

Have any secrets **ever** been exposed? Usually this happens when `secrets.yml` has been committed to the repository.

Any secrets that have been exposed should not be used any longer. 

An exposed secret_token can allow an attacker to gain command line access to your server via your Rails app.

`rake secret` can help with generating new secrets. Ensure these are updated on any affected environments (production, staging, etc.).

## Gem Versions

Run `bundle outdated` to check for old gem versions. Decide appropriate next steps on a gem-by-gem basis.

## Force SSL

```ruby
config/environments/production.rb
config.force_ssl = true
```

The web is moving towards TLS/SSL-on everywhere. Some HTML5 features are not available if your site is not served over SSL. This is the *first* (not only) step you can take towards protecting the good people using your app.

(If your app is hosted on an *.herokuapp.com domain, you get to use their SSL certificate for free, i.e. https://your-app.herokuapp.com just works.)


# Controllers

## Use HTTP Status Code Symbols

Try to avoid magic numbers, favor Rails' symbols for specifying statuses as they describe the purpose of the status code.

```ruby
render json: new_user, status: 200, root: false # Avoid magic numbers
render json: new_user, status: :created, root: false # Good!
```

status code | rails symbol
------------|-------------
200 | :ok
201 | :created
422 | :unprocessable_entity

More symbol values available from the individual pages indexed here: http://httpstatus.es/

# Presentation & Accessibility

## Good Page Titles

Are the page titles specific to the page? Page titles are useful to humans and search engine robots.

Consider using the [`content_for`](http://api.rubyonrails.org/classes/ActionView/Helpers/CaptureHelper.html#method-i-content_for) helper or the [flutie](https://github.com/thoughtbot/flutie) gem.


# Database

## Healthy schema.rb 

Read through `db/schema.rb`. 

- Do the tables and columns have good names?
- Is it indexed appropriately?
- Is the database normalized (i.e. has no duplicate or redundant data)?
- If the database is denormalized, is it for a good reason (e.g. performance)?


# Version Control

## Tidy Commit History

- Are commits focused, small, and made with descriptive, relevant messages?
- Are bugfix and feature branches used?


# Documentation

## Kickstart New Developers

Include up-to-date instructions for how new developers can get started with setting up, running the app and working on it.

Usually these instructions are in `README.md`.

---

# Contributors

- Eliot Sykes https://eliotsykes.com/
- Your name here, contributions are welcome and easy! Just fork the GitHub repo, make your changes, then submit your pull request!

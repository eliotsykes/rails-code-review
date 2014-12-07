# Rails Code Review

An evolving set of guidelines & supporting reasons to consider when code reviewing Ruby on Rails apps.

<!-- MarkdownTOC depth=0 autolink=true bracket=round -->

- [Security](#security)
  - [Keep Rails Vulnerabilities Patched](#keep-rails-vulnerabilities-patched)
  - [Purge Exposed Secrets](#purge-exposed-secrets)
  - [Keep Gem Versions Updated](#keep-gem-versions-updated)
  - [Force SSL](#force-ssl)
- [Controllers](#controllers)
  - [Use HTTP Status Code Symbols](#use-http-status-code-symbols)
- [Presentation & Accessibility](#presentation--accessibility)
  - [Accurate Page Titles](#accurate-page-titles)
- [Database](#database)
  - [Check `schema.rb` is in Good Shape](#check-schemarb-is-in-good-shape)
- [Version Control](#version-control)
  - [Have a Healthy Commit History](#have-a-healthy-commit-history)
- [Documentation](#documentation)
  - [Kickstart New Developers](#kickstart-new-developers)
- [Contributors](#contributors)

<!-- /MarkdownTOC -->


# Security

## Keep Rails Vulnerabilities Patched

Update Rails to the latest patch version. Edit the rails version in the Gemfile then run `bundle install`. Ensure the version of Rails you're on is still actively supported and patched.

## Purge Exposed Secrets

Have any secrets **ever** been exposed? Usually this happens when `secrets.yml` has been committed to the repository.

Any secrets that have been exposed should not be used any longer. 

An exposed `secret_token` can allow an attacker to gain command line access to your server via your Rails app.

`rake secret` can help with generating new secrets. Ensure these are updated on any affected environments (production, staging, etc.).

## Keep Gem Versions Updated

Run `bundle outdated` to check for old gem versions. Decide appropriate next steps on a gem-by-gem basis. Check the CHANGELOG for each gem. Update gems that have had security flaws fixed.

## Force SSL

```ruby
# config/environments/production.rb
config.force_ssl = true
```

The web is moving towards TLS/SSL-on everywhere. Some HTML5 features are not available if your site is not served over SSL. This is one of many steps you will want to take towards protecting the good people using your app.

(If your app is hosted on an *.herokuapp.com domain, you get to use their SSL certificate for free, i.e. https://your-app.herokuapp.com just works.)


# Controllers

## Use HTTP Status Code Symbols

Try to avoid magic numbers. Favor symbols for specifying statuses as they describe the purpose.

```ruby
render json: new_user, status: 201 # Avoid magic numbers
render json: new_user, status: :created  # Good!
```

status code | rails symbol
------------|-------------
200 | :ok
201 | :created
422 | :unprocessable_entity

More symbol values available from the individual pages indexed here: http://httpstatus.es/

# Presentation & Accessibility

## Accurate Page Titles

Are the page titles specific to the page? Page titles are useful to humans and search engine robots.

Consider using the [`content_for`](http://api.rubyonrails.org/classes/ActionView/Helpers/CaptureHelper.html#method-i-content_for) helper or the [flutie](https://github.com/thoughtbot/flutie) gem.


# Database

## Check `schema.rb` is in Good Shape

Read through `db/schema.rb`. 

- Do the tables and columns have good names?
- Is it indexed appropriately?
- Is the database normalized (i.e. has no duplicate or redundant data)?
- If the database is denormalized, is it for a good reason (e.g. performance)?


# Version Control

## Have a Healthy Commit History

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

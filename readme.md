# Soltivo Documentation

## Requirements
Before getting started, make sure your have the following installed on your system:
- [Gem](https://www.ruby-lang.org/en/documentation/installation/)
- [Jekyll](https://jekyllrb.com/docs/installation/)
- [Bundler](https://bundler.io/)

## Get started

1. Open terminal

2. Run `bundle install`

3. Initialize search data (creates search-data.json)
    ```sh
    bundle exec just-the-docs rake search:init
    ```
4. Run your local Jekyll server
    ```sh
    bundle exec jekyll serve
    ```
5. To preview your site, in your web browser, navigate to `http://localhost:4000`.

# Theme used
The current theme is called just-the-docs. Theme's documentation can be found at https://pmarsceill.github.io/just-the-docs/

## Contributions rules
- Make sure your pull from the master branch
- Create a PR
  - Make sure your PR contains the following:
    - A description of the changes made
    - Why those changes were made
- Make sure your code is commented out
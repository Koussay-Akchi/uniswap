# UniSwap

[![CircleCI](https://circleci.com/gh/sharetribe/sharetribe/tree/master.svg?style=svg)](https://circleci.com/gh/sharetribe/sharetribe/tree/master) [![Code Climate](https://codeclimate.com/github/sharetribe/sharetribe.png)](https://codeclimate.com/github/sharetribe/sharetribe)

Uniswap is a new online marketplace created specifically for our university community. It’s a platform where students and teachers can easily buy, sell, or give away used items like textbooks, electronics, furniture, and more. Instead of throwing things away, users can list their unused items for others to find and purchase at affordable prices.

The app is designed to be simple, convenient, and eco-friendly. By reusing items, we help reduce waste and create a more sustainable campus. 


Whether you’re looking to declutter your space, find a bargain, or just connect with others on campus, Uniswap is here to make it easy. Join us in building a community that values sustainability, affordability, and convenience.


### Contents

- [Technology stack](#technology-stack)
- [Installation](#installation)
- [License](#license)

## Technology stack

- Ruby 3.2.2
- Ruby on Rails 6.1.7.3
- MySQL 5.7
- React + jQuery
- Node.js 18.16 (for compiling JavaScript assets)
- "what you see is what you get" Editor [Mercury](http://jejacks0n.github.io/mercury/)
- Deploy: Custom Script (not using Mina or Cap3)
- Server: Heroku
- Image hosting: Amazon S3
- Background job: [delayed_job](https://github.com/collectiveidea/delayed_job)
- Gems:
    -  [devise](https://github.com/plataformatec/devise) | Authentication
    -  [omniauth-facebook](https://github.com/mkdynamic/omniauth-facebook) | Third party login: Facebook
    -  [haml](https://github.com/haml/haml) and ERB | HTML templating
    -  [mysql2](https://github.com/brianmario/mysql2) | MySQL library for Ruby
    -  [paperclip](https://github.com/thoughtbot/paperclip) | Image upload management
    -  [passenger](https://github.com/phusion/passenger) | Web application server
    -  [react_on_rails](https://github.com/shakacode/react_on_rails) | Integration of React + Webpack + Rails
    -  factory_girl, capybara, rspec-rails, cucumber-rails, selenium-webdriver | Testing

## Installation

### Requirements

Before you get started, the following needs to be installed:
  * **Ruby**. Version 3.2.2 is currently used and we don't guarantee everything works with other versions. If you need multiple versions of Ruby, [RVM](https://rvm.io//) or [rbenv](https://github.com/rbenv/rbenv) is recommended.
  * [**RubyGems**](http://rubygems.org/)
  * **Bundler**: `gem install bundler`
  * **Node**. Version 18.16 is currently used and we don't guarantee everything works with other versions. If you need multiple versions of Node, consider using [n](https://github.com/tj/n), [nvm](https://github.com/creationix/nvm), or [nenv](https://github.com/ryuone/nenv).
  * [**Git**](http://help.github.com/git-installation-redirect)
  * **A database**. Only MySQL 5.7 has been tested, so we give no guarantees that other databases (e.g. PostgreSQL) work. You can install MySQL Community Server two ways:
    1. If you are on a Mac, use homebrew: `brew install mysql` (*highly* recommended). Also consider installing the [MySQL Preference Pane](https://dev.mysql.com/doc/refman/5.1/en/osx-installation-prefpane.html) to control MySQL startup and shutdown. It is packaged with the MySQL downloadable installer, but can be easily installed as a stand-alone.
    2. Download a [MySQL installer from here](http://dev.mysql.com/downloads/mysql/)
  * [**Sphinx**](http://pat.github.com/ts/en/installing_sphinx.html). Version 2.1.4 has been used successfully, but newer versions should work as well. Make sure to enable MySQL support. If you're using OS X and have Homebrew installed, install it with `brew install sphinx --with-mysql`
  * [**Imagemagick**](http://www.imagemagick.org). If you're using OS X and have Homebrew installed, install it with `brew install imagemagick`

### Setting up the development environment

1.  Get the code. Clone this git repository and check out the latest release:

    ```bash
    git clone git://github.com/uniswap/uniswap.git
    cd uniswap
    git checkout latest
    ```

1.  Install the required gems by running the following command in the project root directory:

    ```bash
    bundle install
    ```

    **Note:** [`libv8` might fail to build with Clang 7.3](https://github.com/cowboyd/libv8/pull/207), in that case you can try installing V8 manually:

    ```bash
    brew tap homebrew/versions
    brew install v8-315

    gem install libv8 -v '3.16.14.13' -- --with-system-v8
    gem install therubyracer -- --with-v8-dir=/usr/local/opt/v8-315

    bundle install
    ```

1.  Install node modules:

    ```bash
    npm install && ( cd client && npm install )
    ```

1.  Create a `database.yml` file by copying the example database configuration:

    ```bash
    cp config/database.example.yml config/database.yml
    ```

1.  Add your database configuration details to `config/database.yml`. You will probably only need to fill in the password for the database(s).

1.  Create a `config.yml` file by copying the example configuration file:

    ```bash
    cp config/config.example.yml config/config.yml
    ```

1.  Create and initialize the database:

    ```bash
    bundle exec rake db:create db:structure:load
    ```

1.  Run Sphinx index:

    ```bash
    bundle exec rake ts:index
    ```

    **Note:** If your MySQL server is configured for SSL, update the `config/thinking_sphinx.yml` file and uncomment the `mysql_ssl_ca` lines. Configure correct SSL certificate chain for connection to your database over SSL.

1.  Start the Sphinx daemon:

    ```bash
    bundle exec rake ts:start
    ```

1.  Start the development server:

    ```bash
    foreman start -f Procfile.static
    ```

1.  Invoke the delayed job worker in a new console (open the project root folder):

    ```bash
    bundle exec rake jobs:work
    ```


Congratulations! uniswap should now be up and running for development purposes. Open a browser and go to the server URL (e.g. http://lvh.me:3000 or http://lvh.me:5000). Fill in the form to create a new marketplace and admin user. You should be now able to access your marketplace and modify it from the admin area.

### Mailcatcher

Use [Mailcatcher](http://mailcatcher.me) to receive sent emails locally:

1.  Install Mailcatcher:

    ```bash
    gem install mailcatcher
    ```

1.  Start it:

    ```bash
    mailcatcher
    ```

1.  Add the following lines to `config/config.yml`:

    ```yml
    development:
      mail_delivery_method: smtp
      smtp_email_address: "localhost"
      smtp_email_port: 1025
    ```

1.  Open `http://localhost:1080` in your browser

### Database migrations

To update your local database schema to the newest version, run database migrations with:

  ```bash
  bundle exec rake db:migrate
  ```


## License

uniswap is source-available under the sharetribe go Community Public License. See [LICENSE](LICENSE) for details.

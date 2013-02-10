Wordpress on Heroku with Svbtle
===============================

This project is a template for installing and running [WordPress](http://wordpress.org/) on [Heroku](http://www.heroku.com/) with the '[wp-svbtle](https://github.com/gravityonmars/wp-svbtle)' theme. The repository comes bundled with [PostgreSQL for WordPress](http://wordpress.org/extend/plugins/postgresql-for-wordpress/) and [WP Read-Only](http://wordpress.org/extend/plugins/wpro/).

### Updates:

I will detail any updates here.

#### 2013/02/10 - Updated to WordPress 3.5.1

Updated to WordPress 3.5.1 as well as updated to the latest wp-svbtle. I have forked gravityonmars' repo here: [webjames/wp-svbtle](https://github.com/webjames/wp-svbtle) to track the changes i have made. Blog post here: [heroku-wordpress-svbtle Updated to WordPress 3.5.1](http://blog.webjames.co.uk/heroku-wordpress-svbtle-updated-to-wordpress-3-5-1/286/)

#### 2013/01/19 - Enabled email using SendGrid add-on

I have written a blog post about how to set up email using SendGrid, changes have been merged into this repository:

[Enabling email on Wordpress on Heroku using SendGrid](http://blog.webjames.co.uk/enabling-email-on-wordpress-on-heroku-using-sendgrid/266/)

#### 2013/01/13 - Blog post

I have written a blog post about this:

[Hosting a Wordpress Blog on Heroku with the Svbtle Theme for free](http://blog.webjames.co.uk/hosting-a-wordpress-blog-on-heroku-with-the-svbtle-theme-for-free/201/)

### Special Thanks
Special thanks to [gravityonmars](https://github.com/gravityonmars) for [wp-svbtle](https://github.com/gravityonmars/wp-svbtle).

Special thanks to [mhoofman](https://github.com/mhoofman) for [wordpress-heroku](https://github.com/mhoofman/wordpress-heroku).

Installation
============

Clone the repository from Github

    $ git clone git://github.com/webjames/heroku-wordpress-svbtle.git
    
With the [Heroku gem](http://devcenter.heroku.com/articles/heroku-command), create your app

    $ cd heroku-wordpress-svbtle
    $ heroku create
    > Creating strange-turtle-1234... done, stack is cedar
    > http://strange-turtle-1234.herokuapp.com/ | git@heroku.com:strange-turtle-1234.git
    > Git remote heroku added

Add a database to your app

    $ heroku addons:add heroku-postgresql:dev
    > Adding heroku-postgresql:dev to strange-turtle-1234... done, v2 (free)
	> Attached as HEROKU_POSTGRESQL_COLOR
	> Database has been created and is available
	> Use `heroku addons:docs heroku-postgresql:dev` to view documentation

Promote the database (replace COLOR with the color name from the above output)

	$ heroku pg:promote HEROKU_POSTGRESQL_COLOR
	> Promoting HEROKU_POSTGRESQL_COLOR to DATABASE_URL... done

Add SendGrid to your app - optional, to enable email.

    $ heroku addons:add sendgrid:starter

Create a new branch for any configuration/setup changes needed

    $ git checkout -b production
    
Copy the `wp-config.php`

    $ cp wp-config-sample.php wp-config.php

Clear `.gitignore` and commit `wp-config.php`

    $ >.gitignore
    $ git add .
    $ git commit -m "Initial WordPress commit"
    
Deploy to Heroku

    $ git push heroku production:master
    > -----> Heroku receiving push
    > -----> PHP app detected
    > -----> Bundling Apache v2.2.22
    > -----> Bundling PHP v5.3.10
    > -----> Discovering process types
    >        Procfile declares types -> (none)
    >        Default types for PHP   -> web
    > -----> Compiled slug size is 13.8MB
    > -----> Launcing... done, v5
    >        http://strange-turtle-1234.herokuapp.com deployed to Heroku
    >
    > To git@heroku:strange-turtle-1234.git
    > * [new branch]    production -> master 

After deployment WordPress has a few more steps to setup:

Follow the Wordpress setup

Enable the theme from the admin panel

Log in to http://your-app-url.herokuapp.com/wp-svbtle for a new blogging experience!


Media Uploads
===
[WP Read-Only](http://wordpress.org/extend/plugins/wpro/) plugin is included in the repository allowing the use of [S3](http://aws.amazon.com/s3/) for media uploads.

1. Activate the plugin under 'Plugins', if not already activated.
2. Input your Amazon S3 credentials in 'Settings'->'WPRO Settings'.


Usage
========

* Because a file cannot be written to Heroku's file system, updating and installing plugins or themes should be done locally and then pushed to Heroku.
* To run WordPress locally on Mac OS X try [MAMP](http://codex.wordpress.org/Installing_WordPress_Locally_on_Your_Mac_With_MAMP).

Updating
========

Updating your WordPress version is just a matter of merging the updates into
the branch created from the installation.

    $ git pull # Get the latest

Using the same branch name from our installation:

    $ git checkout production
    $ git merge master # Merge latest
    $ git push heroku production:master

WordPress needs to update the database. After push, navigate to:

    http://your-app-url.herokuapp.com/wp-admin

WordPress will prompt for updating the database. After that you'll be good
to go.

If you get 404 errors on viewing single posts, you will need to re-save your 'Permalink Settings' under Settings -> Permalinks. Just hit save, no need to change anything.

Custom Domains
==============

Heroku allows you to add custom domains to your site hosted with them.  To add your custom domain, enter in the follow commands.

    $ heroku domains:add www.example.com
    > Added www.example.com as a custom domain name to myapp.heroku.com

You'll also want to cover the non "www" side of the url.

    $ heroku domains:add example.com
    > Added example.com as a custom domain name to myapp.heroku.com

Once Heroku recognizes your custom domain(s) you'll then need to setup separate DNS A records for the following ip addresses to point to your domain:

    75.101.163.44
    75.101.145.87
    174.129.212.2

Once the DNS A records propagate, then simply test out your change by hitting the url in the browser to make sure you are good to go.  If you are in need of cheap DNS hosting then I would recommend [DNSimple](https://dnsimple.com/r/571e28804df06f).

The last step is updating your WordPress installation to recognize the new domain.  You'll need to open up the WordPress Admin Dashboard and go to Settings --> General.  From there just update the URL for the WordPress address and you're done.

If you find yourself running into problems, there is a guide posted in the Heroku Docs which can be found [here](https://devcenter.heroku.com/articles/custom-domains).


Licenses
========

#### [wp-svbtle](https://github.com/gravityonmars/wp-svbtle)
(The MIT License)

Copyright (c) 2011 Ricardo Rauch <ricardo@gravityonmars.com>

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

#### [Wordpress](http://wordpress.org)
[The WordPress License](http://wordpress.org/about/license/)
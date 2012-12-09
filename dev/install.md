# Install & Setup
    
This guide assumes you are familiar with Linux-based system administration.


<a id="requirements"></a>
## Requirements

1. PHP 5.3+
2. MongoDB 2.0+
3. Apache or equivalent

Required libraries and extensions are detailed under <a href="#server-setup">Server Setup</a>.


<a id="installation"></a>
## Installation

#### Step 1: Get the latest version

<a href="/alpha/download">Download it here</a>. The option to clone it from **github** will be available soon.

#### Step 2: Make sure these paths are writeable by the web server:

1. core/cache/
2. public/images/
3. public/uploads/
4. logs/ (Optional)


<a id="configuration"></a>
## Configuration

Configuration files are defined in <a href="http://www.yaml.org/">YML</a> format. There are two levels of configuration:

1. config/app.yml
2. config/local.yml (environment)

#### App Config (config/app.yml)
	
This file contains options that apply no matter what environment your app runs in. It should be committed to source control and considered the *default* configuration for the entire app.

app.yml looks something like this:

    app:
      debug: false

	domain_routes:
	  admin: admin.example.com
	  alpha: (www.)?example.com
	
	settings:
	  payments:
	    ...
		
Note: In the future, many of these settings will be defined in the Admin UI and stored in the database, but this config file may always hold fallback values.
		
		
#### Local Config (config/local.yml)
		
This file contains options that only apply to the current environment, usually a single instance (i.e. development, staging, or production). It may contain passwords and other secure information and should not be committed to your primary source repository.
		
local.yml looks something like this:
		
	app:
	  debug: true
		
	mongo:
	  user: your_username
	  pass: your_password
	  database: your_database
	  host: localhost
	  options
	    replicaSet: rs0
	  
	cache:
	  enabled: true
	  backend: memcache
	  host: localhost
	
	settings:
	  payments:
	    ...
		  
Options from local.yml will override and append to that of app.yml.

Note: If you prefer another environment name such as "development" or "production", you may specify it in `config/.env`.

	$ echo "development" >> config/.env
	
In that case, `config/local.yml` must be re-named `config/development.yml`. This can be helpful in identifying particular environments from the application, for example:

	{if $request.env == "development"}
		... do something for development only ...
	{/if}


<a id="server-setup"></a>
## Server Setup

#### Apache

The most basic virtual host configuration looks like this:

	<VirtualHost *:80>
		ServerName example.com
		ServerAlias admin.example.com www.example.com other-domain.com
		DocumentRoot /home/your-project-path/public
		<Directory "/home/your-project-path/public">
			AllowOverride All
		</Directory>
	</VirtualHost>

You should point all domains for a particular instance at the `public/` directory. Routing requires the Apache `mod_rewrite` module. The `AllowOverride` directive tells Apache to use the default `public/.htaccess` file which contains these basic rewrite rules:

	 RewriteCond %{REQUEST_FILENAME} !-f # Serve static files directly
	 RewriteRule ^(.*)$ /start.php [QSA,L] # Rewrite everything else to public/start.php
	 
#### Libraries & Extensions

The current version uses the following by default:

- <a href="http://us3.php.net/manual/en/book.image.php">GD</a> for image processing
- <a href="http://php.net/mongo">Mongo</a> native driver
- <a href="http://us2.php.net/memcached">Memcached</a> (Optional)

The following commands sum it up for Debian based servers:

	$ apt-get install php5-dev
	$ apt-get install php5-gd
	$ apt-get install memcached
	$ apt-get install php-pear
	$ pecl install mongo


<a id="database-setup"></a>
## Database Setup

This guide assumes you already have MongoDB running. If not, please refer to <a href="http://docs.mongodb.org/manual/installation/">Installing MongoDB</a>.

Edit `config/local.yml` and configure a mongo database:

	mongo:
	  user: your_username
	  pass: your_password
	  database: your_database
	  host: localhost
	  port: 27017
	  options:
	    replicaSet: replica_set_name
	  
Note: If you're not using a replica set, you may lead out the `options` setting.
	  
That's it. Models will create collections and ensure indexes at run-time.


#### Database Hosting

There are a few MongoDB hosting services such as <a href="http://mongohq.com">MongoHQ</a> and <a href="http://mongolab.com">MongoLab</a>.

Forward also offers <a href="/alpha/download#hosting">managed hosting with MongoDB</a>.


<a id="updating"></a>
## Updating

Download the latest version and replace the `core` folder. You may need to reset permissions on the `core/cache/` directory depending on the way your web server is setup.

If you plan to customize the Admin template and wish to merge your customizations with changes from the latest release, read on...

#### Using Git to upgrade customized templates

The easiest way to keep templates (such as Admin) up to date is to maintain a clean Git repository of the latest version.

Upload the original code to a directory named `fwdcommerce/`, then initialize a Git repository:

	cd fwdcommerce/
	git init
	
Clone the `fwdcommerce` repository to start a new project:

	cd ..
	git clone fwdcommerce/ your-project
	
Rename the original remote:
	
	git remote rename origin fwdcommerce
	
When it's time to upgrade, upload the latest version into `fwdcommerce/` and commit:

	cd fwdcommerce/
	git commit -am "upgraded to latest release"
	
Back to your project, create a new branch to test the upgrade:
	
	cd ../your-project/
	git checkout -b fwdupdate
		
Merge the latest remote code with yours:
	
	git pull fwdcommerce master
	
Resolve conflicts and test before merging back into the master branch.

	git checkout master
	git merge --no-ff fwdupdate
	git branch -d fwdupdate
	
Using this workflow allows you to upgrade customized templates more easily, but it may not be ideal in every scenario.
		

<a id="help-support"></a>
## Help & Support

If you're having trouble with any of this, don't hesitate to ask for help in the <a href="/alpha/forums">community forums</a>. If that's not enough, consider a <a href="/alpha/download#support">support subscription</a>.
	
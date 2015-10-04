# Ansible Laravel Update

This is a role that I use to update my Laravel 5 applications. It could be made to fit Laravel 4 with minimal effort, but at the moment, I don't have any that require/can use ansible.

---

### Limitations
At the moment, this role suits my needs as they stand so:

- MySQL
- Gulp
- Bower

This is not to say that it couldn't be adapted, so it should be easy enough to swap `Gulp` for `Grunt` and `Mysq` for `Postgres` if requested

---

### Steps taken in role
This is all documented in the `tasks/main.yml`, but it never hurts to list them again.

##### Update Composer
Ensure that the latest version of `composer` is installed.

##### Bring site down
Put the site into maintenance mode to prevent any data changes while the update is occurring

##### Create back up MYSQL dump
Not matter how well I code something, I am still petrified of losing data. This step will copy the database to a local file on your computer at the playbook level.


##### Backup current folder to archive
Same as the database. If the deployment fails for any reason, the site should be able to reverted quickly and easily. This will eventually be made into a role as well.

##### Update from Git
Pull for the latest version.

##### Update folder permissions
Again, no matter how sure I am that I have set the permissions on the folders correctly, getting that white screen still happens sporadically, so this takes care of that. 

##### Add the environment file
Dump everything needed into the `.env` file

##### Update Composer pacakges
This only installs not updates the packages, so you should commit  your `composer.lock` file, especially on low spec servers ( i.e ones that have less that 1 gig of RAM)

##### Update Node modules
Install required NodeJs modules. This is run as the `web_user`.

##### Update Bower
Update the `Bower` packages. This is run as the `web_user`

##### Build static
Build the static files by running `Gulp`. This is run as the `web_user`

##### Optimize Laravel application
Creates a single file containing all the classes with the comments removed.

##### Clear caches
Reset any caches to avoid incorrect data.

##### Migrate database
Make any structural changes to the database.

##### Bring site back up
Brings the site out of maintenance mode.

----

### Variables
Most of these are pretty self explanatory and examples of all variables can be found in `defaults/main.yml`. This contains sensitive data **So ensure you `vault` your var file.

####Database Variables
##### db_host 
This is used in the task, but it can also be used in the .env file. 

##### db_name
This is used in the task, but it can also be used in the .env file. 

##### db_user
This is used in the task, but it can also be used in the .env file. 

##### db_password
This is used in the task, but it can also be used in the .env file. 

##### db_dump_file
The name you want the .sql dump to be saved as.

#### Website Variables
##### web_user
The user who requires permissions for all the files in the app. Usually something like `www`.
##### web_group
The user group who requires permissions for all the files in the app. Usually something like `www`.

##### site_current_folder
The absolute path of the Laravel application.

##### site_archive_folder
The absolute path of the where you want to backup your Laravel application.

##### site_repository
The git repository to pull from. Make sure that you have added your github token / whatever else you need to do.

##### env
Every thing that you need to put in your `.env` file.
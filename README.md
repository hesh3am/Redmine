# Redmine

![redmine_logo](https://github.com/hesh3am/Redmine/assets/34006266/ec5b7369-14d2-419f-875b-25e0a4a80445)

Redmine is a free and open source, web-based project management and issue tracking tool. It allows users to manage multiple projects and associated subprojects. It features per project wikis and forums, time tracking, and flexible, role-based access control.

![redmine](https://github.com/hesh3am/Redmine/assets/34006266/06fa0836-3303-4a31-858f-e26c12d4385b)

Where to Store Data
Important note: There are several ways to store data used by applications that run in Docker containers. We encourage users of the redmine images to familiarize themselves with the options available, including:

Let Docker manage the storage of your files by writing the files to disk on the host system using its own internal volume management. This is the default and is easy and fairly transparent to the user. The downside is that the files may be hard to locate for tools and applications that run directly on the host system, i.e. outside containers.
Create a data directory on the host system (outside the container) and mount this to a directory visible from inside the container. This places the database files in a known location on the host system, and makes it easy for tools and applications on the host system to access the files. The downside is that the user needs to make sure that the directory exists, and that e.g. directory permissions and other security mechanisms on the host system are set up correctly.
The Docker documentation is a good starting point for understanding the different storage options and variations, and there are multiple blogs and forum postings that discuss and give advice in this area. We will simply show the basic procedure here for the latter option above:

Create a data directory on a suitable volume on your host system, e.g. /my/own/datadir.

Start your redmine container like this:

$ docker run -d --name some-redmine -v /my/own/datadir:/usr/src/redmine/files --link some-postgres:postgres redmine
The -v /my/own/datadir:/usr/src/redmine/files part of the command mounts the /my/own/datadir directory from the underlying host system as /usr/src/redmine/files inside the container, where Redmine will store uploaded files.

Port Mapping
If you'd like to be able to access the instance from the host without the container's IP, standard port mappings can be used. Just add -p 3000:3000 to the docker run arguments and then access either http://localhost:3000 or http://host-ip:3000 in a browser.

Environment Variables
When you start the redmine image, you can adjust the configuration of the instance by passing one or more environment variables on the docker run command line.

REDMINE_DB_MYSQL, REDMINE_DB_POSTGRES, or REDMINE_DB_SQLSERVER
These variables allow you to set the hostname or IP address of the MySQL, PostgreSQL, or Microsoft SQL host, respectively. These values are mutually exclusive so it is undefined behavior if any two are set. If no variable is set, the image will fall back to using SQLite.

REDMINE_DB_PORT
This variable allows you to specify a custom database connection port. If unspecified, it will default to the regular connection ports: 3306 for MySQL, 5432 for PostgreSQL, and empty string for SQLite.

REDMINE_DB_USERNAME
This variable sets the user that Redmine and any rake tasks use to connect to the specified database. If unspecified, it will default to root for MySQL, postgres for PostgreSQL, or redmine for SQLite.

REDMINE_DB_PASSWORD
This variable sets the password that the specified user will use in connecting to the database. There is no default value.

REDMINE_DB_DATABASE
This variable sets the database that Redmine will use in the specified database server. If not specified, it will default to redmine for MySQL, the value of REDMINE_DB_USERNAME for PostgreSQL, or sqlite/redmine.db for SQLite.

REDMINE_DB_ENCODING
This variable sets the character encoding to use when connecting to the database server. If unspecified, it will use the default for the mysql2 library (UTF-8) for MySQL, utf8 for PostgreSQL, or utf8 for SQLite.

REDMINE_NO_DB_MIGRATE
This variable allows you to control if rake db:migrate is run on container start. Just set the variable to a non-empty string like 1 or true and the migrate script will not automatically run on container start.

db:migrate will also not run if you start your image with something other than the default CMD, like bash. See the current docker-entrypoint.sh in your image for details.

REDMINE_PLUGINS_MIGRATE
This variable allows you to control if rake redmine:plugins:migrate is run on container start. Just set the variable to a non-empty string like 1 or true and the migrate script will be automatically run on every container start. It will be run after db:migrate.

redmine:plugins:migrate will not run if you start your image with something other than the default CMD, like bash. See the current docker-entrypoint.sh in your image for details.

REDMINE_SECRET_KEY_BASE
This variable is required when using Docker Swarm replicas to maintain session connections when being loadbalanced between containers. It will create an initial config/secrets.yml and set the secret_key_base value, which is "used by Rails to encode cookies storing session data thus preventing their tampering. Generating a new secret token invalidates all existing sessions after restart" (session store). If you do not set this variable or provide a secrets.yml one will be generated using rake generate_secret_token.

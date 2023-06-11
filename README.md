# Redmine

![redmine_logo](https://github.com/hesh3am/Redmine/assets/34006266/ec5b7369-14d2-419f-875b-25e0a4a80445)

Redmine is a free and open source, web-based project management and issue tracking tool. It allows users to manage multiple projects and associated subprojects. It features per project wikis and forums, time tracking, and flexible, role-based access control.

![redmine](https://github.com/hesh3am/Redmine/assets/34006266/06fa0836-3303-4a31-858f-e26c12d4385b)

Where to Store Data
Important note: There are several ways to store data used by applications that run in Docker containers. We encourage users of the redmine images to familiarize themselves with the options available, including: MYSQL

Let Docker manage the storage of your files by writing the files to disk on the host system using its own internal volume management. This is the default and is easy and fairly transparent to the user. The downside is that the files may be hard to locate for tools and applications that run directly on the host system, i.e. outside containers.
Create a data directory on the host system (outside the container) and mount this to a directory visible from inside the container. This places the database files in a known location on the host system, and makes it easy for tools and applications on the host system to access the files. The downside is that the user needs to make sure that the directory exists, and that e.g. directory permissions and other security mechanisms on the host system are set up correctly.
The Docker documentation is a good starting point for understanding the different storage options and variations, and there are multiple blogs and forum postings that discuss and give advice in this area. We will simply show the basic procedure here for the latter option above:


# ![Installfest- Python, Django, SQL - Ubuntu and WSL](./assets/hero.png)

![The macOS Installfest Logo](./assets/installfest-logo-ubuntu.png)

## Python 3

To install Python,

```
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:deadsnakes/ppa
$ sudo apt-get update
$ sudo apt-get install python3.11
```

You can test the installation by running `python3 --version`.

# PostgreSQL

PostgreSQL is a popular and robust Relational Database Management System (RDBMS).

Check if PostgreSQL is already installed by running this command:

```
psql
```

If you entered PostgreSQL's Interactive Shell, you already have PostgreSQL installed. Enter \q to exit the shell.

If you already have PostgreSQL installed, note the version and inform an instructor if it's not at least version 10.

Otherwise, let's install it:

### Install PostgreSQL

First, make sure your system packages are up to date. In the terminal:

```bash
sudo apt update
```

Next, run the following command to install PostgreSQL:

```bash
sudo apt install postgresql postgresql-contrib
```

You may be asked if you would like to continue. Type "Y" and hit the "Enter" key to continue installing.

**Important!!** You'll have to run this command _on each restart_ to start up the PostgreSQL service:

```bash
sudo service postgresql start
```

### The _first time_ you start this service:

You must assign a password to the default admin user to connect to a database. Do so with this command:

```bash
sudo passwd postgres
```

Enter a password when prompted. It is _EXTREMELY IMPORTANT_ that you do not forget the password. Keep it stored in your password manager. Note that as you type, no characters will appear.

If the new password you have chosen is valid you will see this returned in the console:

```
passwd: password updated successfully
```

Upon seeing this, stop the PostgreSQL service with this command:

```bash
sudo service postgresql stop
```

Close all running terminal instances including any instances of VS Code that are currently open.

Open a new terminal instance. Start the PostgreSQL service again with:

```bash
sudo service postgresql start
```

We now need to create a database role that matches the name of your operating system user. Use this command (do not replace `$USER`, this will automatically create a user that matches your Ubuntu OS username):

```bash
sudo -u postgres createuser $USER
```

And then follow that up by creating a database of the same name (again, do not replace $USER, this will automatically create a user that matches your Ubuntu OS username):

```bash
sudo -u postgres createdb $USER
```

Now run this command to switch to the postgres user:

```bash
sudo su - postgres
```

Finally, enter:

```bash
psql
```

You have now successfully entered the PostgreSQLshell as the postgres user!

### Assigning Roles

We want the user we just made to be able to create databases so we need to assign it that role with this command in the postgres shell. **YOU MUST replace `<user>` with your Ubuntu username:**

```
ALTER ROLE <user> WITH CREATEDB;
```

Don't forget the semicolon ^

You may want to also give this user super user permissions. This will keep you from having to ever use the postgres user again, but will strip a layer of protection from your postgres databases. This choice is ultimately yours, but if you do want to give your user account these permissions run this command. **YOU MUST replace `<user>` with your Ubuntu username:**

```
ALTER ROLE <user> WITH SUPERUSER;
```

Again, don't forget the semicolon ^

Close the terminal session, and start a new one.

You should now be able to run this command and start the Postgres shell successfully:

```
psql
```

Let's test your ability to create databases. Run the following command in the postgres shell:

```
CREATE DATABASE apples;
```

As always, don't leave off the semicolon!

Next, enter the `\l` command to see a list of all databases. If you can see the `apples` database you just created, then success!

Let's go ahead and delete that database with the following command:

```
DROP DATABASE apples;
```

Enter `\l` again to confirm the database was dropped.


## Installing Django in a virtual environment

### What is a Virtual Environment?

A virtual environment is a self-contained directory that contains a Python interpreter along with all the libraries and dependencies needed for a particular project. It allows you to isolate your project's dependencies from other projects on your system, preventing conflicts between different versions of the same library and ensuring that your project runs consistently across different environments.

### How Virtual Environments Work with Django:

Django is a powerful web framework for building web applications using Python. When developing Django applications, you often need to install additional packages and libraries to extend its functionality. Virtual environments provide a clean and isolated environment where you can install these dependencies without affecting other projects or the system-wide Python installation.

### By creating a virtual environment for each Django project, you can:

- Install Django and its dependencies locally within the environment.
- Install additional packages required for your project, such as database drivers, middleware, or third-party apps.
- Ensure that your project remains compatible with specific versions of Django and other dependencies.
- Easily manage and update dependencies without worrying about conflicts with other projects.

### Creating a virtual environment


Update your package list:

```bash
sudo apt update
```

Install pip (Python package installer):

```bash
sudo apt install python3-pip
```

Install [pipenv](https://pypi.org/project/pipenv/) using pip:

```bash
pip install --user pipenv
```

Navigate to the directory where you want to create your project.

Create a new directory for a test project (optional):

```bash
mkdir my_test_project
cd my_test_project
```
Initialize a new virtual environment inside your project directory:

```bash
pipenv install django
```

This command will create a new `Pipfile` and `Pipfile.lock` in your project directory, specifying Django as a dependency.

Activate the pipenv shell:

```bash
pipenv shell
```

This will activate the virtual environment and change your terminal prompt to indicate that you are now working within the pipenv shell.

Verify the installation:

```bash
django-admin --version
```

When you're done working on your project, you can deactivate the pipenv shell by simply typing:

```bash
exit
```
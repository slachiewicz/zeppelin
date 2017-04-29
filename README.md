# Zeppelin for SQL Server

This fork of Apache Zeppelin is focused on specific support for SQL Server and SQL Azure. Please refer to Apache Zeppelin main page for general information on the project:

- [Apache Zeppelin](https://zeppelin.apache.org/)
- [GitHub Page](https://github.com/apache/zeppelin)

## Project Status

[![Build Status](https://travis-ci.org/yorek/zeppelin.svg?branch=v0.7)](https://travis-ci.org/yorek/zeppelin)

## Requirements
 * Java 1.8
 * Tested and Build on Ubuntu 16.04 LTS
 * Maven (if you want to build from the source code)
 * Node.js Package Manager (npm, downloaded by Maven during build phase)

## Running Apache Zeppelin

If you don't want to *build* Apache Zeppelin, but you just want to use it, you have two options. In both cases once you have Apache Zeppelin running, read the "Using Apache Zeppelin" section below to quickly start playing with it.

### Using a Docker Image

If you're into docker and/or you don't want to install Java on your machine, you can use the Docker Image here

[Apache Zeppelin for SQL Server Docker Image](https://github.com/yorek/zeppelin-sqlserver-docker)

### Native installation on Windows 10

If you prefer to install it directly on your machine read the document here:

[Running Apache Zeppelin for SQL Server on Windows 10 natively](https://medium.com/apache-zeppelin-stories/running-apache-zeppelin-for-sql-server-on-windows-10-natively-3fe2e6d9baa5)

## Building Apache Zeppelin

### Before Build
The installation method may vary according to your environment, example is for Ubuntu 16.04 LTS 64bits.
You can download Ubuntu from here: http://www.ubuntu.com/download/desktop/.

The current version has been built and tested on Ubuntu 16.04 LTS 64bits.

From a terminal shell:

```
# install packages
sudo apt-get update
sudo apt-get install git
sudo apt-get install openjdk-8-jdk
sudo apt-get install nodejs
sudo apt-get install npm
sudo apt-get install libfontconfig
sudo apt-get install maven
```

### Get Source Code

Download code from GitHub. From a terminal shell:

```
git clone --branch=v0.7 https://github.com/yorek/zeppelin.git zeppelin-sqlserver
```

This will clone the GitHub repository into a folder named ```zeppelin-sqlserver``` in your home directory

### Build

From a terminal shell:

```
cd ~/zeppelin-sqlserver

export MAVEN_OPTS="-Xmx2g"

mvn clean package -DskipTests

cp ./conf/zeppelin-site.xml.template ./conf/zeppelin-site.xml
cp ./conf/zeppelin-env.sh.template ./conf/zeppelin-env.sh
```

Please note that the above commands already contains anything needed in order to make Zeppelin work with SQL Server.
If you want to have more information on the SQL Server interpreter, you can take a look at the readme in the ```sqlserver``` folder:

[SQL Server Interpreter for Apache Zeppelin](https://github.com/yorek/zeppelin/sqlserver/README.md)

### Configure

If you wish to configure Zeppelin option (like port number), configure the following files:

```
./conf/zeppelin-env.sh
./conf/zeppelin-site.xml
```

### Start Zeppelin

From a terminal shell, start Zeppelin Daemon:

```
./bin/zeppelin-daemon.sh start
```

you can now head to ```http://localhost:8080``` to see Zeppelin running.

## Using Apache Zeppelin

### Create and configure the Interpreter

Open the Interpreter window by clicking on the "Anonymous" item on the to right and selecing the "Interprepter" menu item so that Zeppelin will show you the Interpreters configuration page.

#### Change an existing configuration

Scroll to the bottom of the page to find the ```sqlserver``` interpreter. Click on the ```edit``` button on the right and fill the properties with the values correct for the SQL Server or SQL Azure instance you'd like to connect to. The property ```sqlserver.driver.name``` is already set to the correct value. Change it *only* if you really know what you're doing.

the ```sqlserver.url``` parameter is more or less the equivalent of a connection string in .NET. To connect to a local SQL Server it will be something like:

```
jdbc:sqlserver://<your-local-sql-server-address>:1433
```

to connect to SQL Azure or SQL Azure DW it will be similar to:

```
jdbc:sqlserver://<your-sql-azure-server-name>.database.windows.net:1433
```

Now click on save and now you're ready to use the configured SQL Server interpreter in a Notebook.

#### Create a new configuration

If you want to create a new SQL Server interpreter to connect to a different SQL Server, just click on the ```+ Create``` button on the top right at the beginning of the page. Type a name for your interpreter, for example, "SQL Server" and from the interpreter drop-down menu select the ```sqlserver``` item. Now you can follow the same procedure described above to configure your new interpreter.

### Creating a Notebook

On the ```Notebook``` menu, select the ```+ Create new note``` item. Give the notebook the name you prefer, for example "SQL Azure".

Now you have to choose which interpreter you want to use among all the ones available. To do so, click on the gear icon on the right, near the ```default``` button.
The selected interpreter, which will be available to use in your notebook, will be in light blue. The deselected one will be shown in light gray. You should have all the interpreter already selected. If you want to change something, click one the interpreter you want to enable or disable to do so. Just make sure that the ```sqlserver``` interpreter is selected. Save your choices by pressing on the ```Save``` button.

### Using a Notebook

Now click on the white box on the top, and you'll be able to write your first query. Something like:

```
%sqlserver
select @@version
```

will be enough to make sure that SQL Server interpreter is working correctly. The first line tells Zeppelin that you're going to send something that has to be interpreted by the SQL Server Interpreter. 
The second one simply ask to SQL Server to return server name and version info. The first line could also be omitted if the SQL Server Interpreter is the first in the list of interpreters bound the the active notebook.
Tu run the code, just hit ```Shift + Enter```

Welcome to the Apache Zeppelin world!

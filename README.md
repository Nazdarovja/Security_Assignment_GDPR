# Hacking Royale : "There can only be one"
This is a semester project for Security course. This project is created as a "game" where you have to hack to advance.

![overview image](overview.png)

## Project idea:
    
    Create app / applications, which progressively have security flaws based on the OWASP top 10, with supplied hints.(some in the app other must be supplied)

    (Prepare short oral examples to how these vulnerabilities could be avoided.)

##  Getting started
The supplied instructions will get the project up and running locally, but for the project to function it should be run on a linux machine.
For our example we used a cheap droplet on Digital Ocean running Linux Ubuntu 18.4, one should also create a user without `sudo` privileges to the server.

(Some of the hacks makes the user exposed to the hacker, and to limit the damage we recommend using a regular and not `sudo` user)
Further more there should be created a MySQL database and also user to the database(also with some restrictions).

### Prerequisites

This project is entirely built in NodeJS and express.js
As mentioned above, this project should be deployed on a linux machine(a virtual machine will do as well).

```
- Prepare a linux server/ virtual machine to run the application on (if you run linux i would still not recommend running it on your machine, some of the hacks expose it completely)
- Create a user without sudo privileges (Important!)
```

First: You have to install MySQL server on your linux installation
``` 
$apt-get mysql-server
create new mysql user
create new database named security_database
then run the script provided in \part1and2\db_script.sql
(the provided two users are for test purposes, and should be removed when running the game)
```

Second: Install Node.js on the server/ only ubuntu guide is provided

```
Install NodeJS following guide in this link : 
https://tecadmin.net/install-latest-nodejs-npm-on-ubuntu/
```

Third: For running the application when deployed install a process manager; pm2

```
$sudo npm install pm2 -g
```

Fourth: Install whois, that is required for second part
```
$sudo apt-get install whois
```

### Installing

A step by step series of examples that tell you how to get a development env running

First clone the project, this project is divided in two folders

Go to root folder of the project then run following commands:
```
install dependencies for the projects
$cd part1and2
$npm install (yarn will work as well)
$cd ../part3
$npm install
```

Next you have to supply login info for MySQL

Go to part1and2 folder and add your credentials to the dbfacade.js file:
```
$cd part1and2/facade/dbfacade.js
change credentails to your own
```

Now the project is ready to run (as two seperate projects on the server)
(The second part and fourth/demo part will only work on a macOS/ or linux)
```
For Part 1 and 2
$cd part1and2
$npm start

For Part 3
$cd part3
$npm start
```
### Deployment
When you have set up the code with your own MySQL credentails(deployed MySQL credentials), and have added an url to the "SUPER_SECRET_FILE", you can deploy and run the application.

First
```
scp or clone the repo to your server

Next go to project root folder and run following commands
$cd part1and2
$pm2 start bin/www
- will be run on port 3000 by default

$cd ../part3
$npm start
- will be run on port 3333 by default
```
Now the application is running, the first and second part will be managed by the process manager(remember to stop it after using it because it is really hackable).
The third part is not setup to restart, because it is even more hackable and is victim to code injection, so the server can be hijacked via your application. Also this serves the purpose that you can see who wins the race, because the server will be terminated, and the last print is from the winner. For a class demo, run part3 from a terminal, mirrored to a projector, so you and the class can see the progress.


# Short assignment descriptions
These are the assignments for the game and an extra demo which demonstrates a more "scary" hack.

## Part One SQL Injection / Broken Access Control / Security Misconfiguration
- Login site, here it should be possible to register new user, and get to a site for regular users where there is a search bar to search for other users.
- Next the user should use SQL Injections and elevate their role, to admin.
- And lastly user should re-login to get to the admin site (logging out in terms of going to the login page and login again.
There is no actual login state).

## Part Two Command Injection
- A simple site with a DNS lookup, where you can get some info about other domains.
- This happens through a "child_process", thus executing commands directly in the terminal, without sanitizing.
- There is then supplied a file ("SUPER_SECRET_FILE") in the root folder of the running project(part1and2) with the URL to the next assignment. 

## Part Three Insecure Deserialization / Code Injection / Using components with known vulnerabilities
- A very simple site, that on the first entry returns a cookie, with some user info. The purpose is to terminate the underlying node express server, with the cookie, which is deserialized on the server.
- The deserialization happens in a sanitized manner, but not properly sanitized so there can happen code injection.
- This is the cookie content : {"username":"CHANGE_ME_TO_YOUR_NAME_THIS_IS_IMPORTANT", "exec": "_$$ND_FUNC$$_console.log('wonder what this does?')"}
- The winner is the one that terminates the node server

## Part Four demonstration of Command Injection through deserialized backdoor, created via code injection.
- Create a backdoor(code is supplied in the app.js file in part3)
- Use linux with netcat to connect to the backdoor and execute commands on the server.
- Guide is supplied in hints document

### *Hints to supply when running game*
[Hints](hints.md)

## Info
https://www.youtube.com/watch?v=qLIkGJrMY9k <- could have been relevant for Task 2
https://www.owasp.org/index.php/Command_Injection

## Authors
Group 4

* **Alexander W. Hørsted-Andersen** - *developer* - [awha86](https://github.com/awha86)
* **Mathias Bigler** - *developer* - [Zurina](https://github.com/Zurina)
* **Mikkel Emil Larsen** - *developer* - [mikkel7emil](https://github.com/mikkel7emil)
* **Stanislav Novitski** - *developer* - [Stani2980](https://github.com/Stani2980)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details

#Loopback API / Heroku server / Mongo database / Git
coucou
##Requirements
--> Install Loopback : 

    $ npm install -g loopback-cli
    
--> Heroku account : sign up at <https://signup.heroku.com/?c=70130000001x9jFAAQ>.  
--> MongoDB account : sign up at <https://mlab.com/signup/>

##Loopback
###Create Loopback server
Open your cmd.  
Choise a location for your new application:


    $ lb

Follow the instructions : name (*"MyApp"*), repository (*"MyAppRepo"*), version and kind of app.  

Go into the repository you just created:  


	$ cd My_app_repo

Start the serveur :

	$ node .

Open a web browser, copy and past "local host" adress.

Each time you want to leave the server, press: **Ctrl + c** twice.  
###Create model
On your cmd:


	$ lb model
Follow the instructions : name (*"MyModel"*), model class, properties ...  
Enjoy your local database !


##Heroku
On your cmd, login Heroku:


	$ heroku login

Create a new server for your app:

	$ heroku create nameOfMyServer

Open Loopback server folder you created in the first step (*"MyAppRepo"* folder).
Create a new file called *"Procfile"* and write on it (Heroku needs it to know which coding language you use):


	web: node server/server.js

Inside the folder nammed *"server"*, create a new file called **"datasources.heroku.js"**.  Copy and paste inside the following code:


    'use strict';
    module.exports = {
        mongo: {url: process.env.MONGO_URL}
    };

##Git
Git allows you to handle your project (further informations: <https://git-scm.com/>).  
Create a new Git repository (in your cmd):  


    $ git init

Add files from the working directory to the staging area


    $ git add .

Permanently stores file changes from the staging area in the repository:


    $ git commit

Sum up modifications, then "write and quit":


    $ :wq

Configure your app with heroku:


    $ heroku config:set --app nameOfMyServer NODE_ENV=heroku

To add your Heroku app as a Git remote, execute:


    $ heroku git:remote -a nameOfMyServer

Push the local branch to Heroku master branch remote:


    $ git push heroku master

*Enjoy your online server:*


    $ heroku open

Or go to the link : <http://NameOfMyApp.herokuapp.com/explorer>


##Mongo database
###Create a database
MongoDB allows you to upload your data.  
Log on to <https://mlab.com/home>.  
In MongoDB Deployments section, click on "Create New" button to create a new deployment.  
Create your database. 
###Generate a database user
Click on "Users" and add database user: name (*"MyUserName"*), password (*"MyPassword"*).

###Connect the database to your application
Inside the folder nammed *"server"*, open *"datasources.json"* file.  
Copy and paste the missing code:


    {
        "db": {
            "name": "db",
            "connector": "memory"
        },

        "mongodb": {
            "url": "mongodb://<MyUserName>:<MyPassword>@ds061148.mongolab.com:61148/tuto-server",
            "name": "mongodb",
            "connector": "loopback-connector-mongodb"
        }
    }

*Make sure you copy and paste the url from MongoDB website and wrote the name and password related to database user you created on MongoLab website.*


![MongoDB]
(https://raw.githubusercontent.com/davidblochQ4U/Tutorial-LoopBack-Heroku-MongoDB/master/mongolab_url.png)

Check in "model-config.json" file that your models are linked to MongoDB:


	"MyModel1": {
        "dataSource": "mongo",
        "public": true
    }
    
Add Mongo connector :


    $ npm install loopback-connector-mongodb --save

Connect your Mongo database to heroku:

	$ heroku config:set --app nameOfMyServer MONGO_URL=mongodb://MyUserName:MyPassword@ds061148.mongolab.com:61148/tuto-server

Start your online server:

    $ nodemon

#  Authentication and Security

## Table of Contents
- [Authentication](#authentication)
- [Authentication Levels](#authentication-levels)
- [Level 1: Email and Password](#level-1-email-and-password)
- [Level 2: Database Encryption and Enviornment variable usage](#level-2-database-encryption-and-enviornment-variable-usage)
- [Level 3: Hashing](#level-3-hashing)
- [Level 4: Hashing and Salting](#level-4-hashing-and-salting)
- [Level 5: Cookies and Sessions or Token Authentication](#level-5-cookies-and-sessions-or-token-authentication)
- [Level 6: OAuth](#level-6-oauth)
- [Example codes](#example-codes)

## Authentication
Authentication is the process that companies use to confirm that only the right people, services, and apps with the right permissions can get organizational resources. It’s an important part of cybersecurity because a bad actor’s number one priority is to gain unauthorized access to systems. They do this by stealing the username and passwords of users that do have access. The authentication process includes three primary steps:

Identification: Users establish who they are typically through a username.
Authentication: Typically, users prove they are who they say they are by entering a password (something only the user is supposed to know), but to strengthen security, many organizations also require that they prove their identity with something they have (a phone or token device) or something they are (fingerprint or face scan).
Authorization: The system verifies that the users have permission to the system that they’re attempting to access.

## Authentication levels
- Level 1: Email and Password
- Level 2: Database Encryption and Enviornment variable usage (using mongoose-encryption, dotenv)
- Level 3: Hashing (using md5)
- Level 4: Hashing and Salting (bcrypt)
- Level 5: Cookies and Sessions (using passport, passport-local, passport-local-mongoose, express-session) or Token authentication
- Level 6: OAuth (using passport-google-oauth2 [may vary depending on Oauth provider])

## Level 1: Email and Password
Username or Email or Mobile Number + Password can be the basic authentication setup.
With plain text being stored in DB, if a hacker gets access to DB, then all the users credentials are at risk.

## Level 2: Database Encryption and Enviornment variable usage
By encryption of the password and by storing sensitive information on a .env file, we improve the security one step further.
However, with growing computational capabilities, the passwords can be cracked. And by physically gaining access to the system with .env file, we get the encryption secret which is common for all the users in DB, and then we can decrypt all the credentials.
We must build a system, where one must be never be able to decrypt the creds even if DB access is compromised.
``` js
// imports
require('dotenv').config()
const express = require('express');
const bodyParser = require('body-parser');
const ejs = require('ejs');

const encrypt = require('mongoose-encryption'); //ONLY ENCRYPTION

const app = express();
// middlewares
app.use(express.static("public"));
app.set('view engine','ejs');
app.use(bodyParser.urlencoded({
    extended:true
}))

// mongoose connection to cloud mongo db
mongoURI="mongodb+srv://<username>:<password>@cluster0.pmyuqmx.mongodb.net/<database>"
mongoose.connect(mongoURI, {
    useNewUrlParser: true,
  })

// mongo db user object definition
const userSchema = new mongoose.Schema({
    username:String,
    password:String
}); 

// encryption plugin (basic encryption)
secret =process.env.SECRET
userSchema.plugin(encrypt,{secret:secret,encryptedFields:['password']})

```

## Level 3: Hashing
Hashing refers to the process of generating a fixed-size output from an input of variable size using the mathematical formulas known as hash functions. 
By hasing password, we get a non decryptable credential which will be stored in DB.
However, since same inputs will always produce same output, this allows the use of a lookup hash table for common passwords. Just by googling such hash values, we can get the passwords.
``` js
// imports
require('dotenv').config()
const express = require('express');
const bodyParser = require('body-parser');
const ejs = require('ejs');
const md5 = require('md5'); //ONLY HASHING

const app = express();
// middlewares
app.use(express.static("public"));
app.set('view engine','ejs');
app.use(bodyParser.urlencoded({
    extended:true
}))

// mongoose connection to cloud mongo db
mongoURI="mongodb+srv://<username>:<password>@cluster0.pmyuqmx.mongodb.net/<database>"
mongoose.connect(mongoURI, {
    useNewUrlParser: true,
  })

// mongo db user object definition
const userSchema = new mongoose.Schema({
    username:String,
    password:String
}); 

// while registering
const newUser = new User({
    email:req.body.username,
    password:md5(req.body.password) //ONLY HASHING
})

// while login
if(md5(passwordEntered)===password){
    // some code
}
```

## Level 4: Hashing and Salting
Salting is the process of adding unique, random strings (salts) to the end of user's password, which will be stored separately in DB and then hashing it.
This salting and hashing can be done many rounds which will make the hashing more difficult to be decrypted.

``` js
// imports
require('dotenv').config()
const express = require('express');
const bodyParser = require('body-parser');
const ejs = require('ejs');
const bcrypt = require('bcrypt'); //HASING AND SALTING
const saltRounds=10;

const app = express();
// middlewares
app.use(express.static("public"));
app.set('view engine','ejs');
app.use(bodyParser.urlencoded({
    extended:true
}))

// mongoose connection to cloud mongo db
mongoURI="mongodb+srv://<username>:<password>@cluster0.pmyuqmx.mongodb.net/<database>"
mongoose.connect(mongoURI, {
    useNewUrlParser: true,
  })

// mongo db user object definition
const userSchema = new mongoose.Schema({
    username:String,
    password:String
}); 

// While registration
bcrypt.hash(req.body.password,saltRounds,async (err,hash)=>{
        if(err){
            res.send({error:err.message});
        }
        const newUser = new User({
            email:req.body.username,
            password:hash //Hashing and salting
        })
        try{
            const result = await newUser.save()
            res.render("secrets")
        }
        catch(e){
            res.status(500).send({error:e.message});
        }
    })

// while login
bcrypt.compare(passwordEntered,password,(err,result)=>{

    if(result){
        res.render("secrets")
    }
    else{
        res.status(401).send('password is wrong')
    }
})
```

## Level 5: Cookies and Sessions or Token Authentication

### Session cookie and Access token
A session cookie and an access token are both ways of authenticating a user’s requests over the internet. They are issued by the server when the user logs in and are sent back with every request. However, they have some differences:

### Session Cookie
A session cookie is a small file that contains a unique ID for the user. The server stores the rest of the user’s information in a session file, which is linked to the ID. The server can access the session file by looking up the ID from the cookie. A session cookie is stored only on the client’s local storage.

### Access Token
An access token is a file that contains the user’s information and permissions, as well as a signature from the server. The server can verify the token by checking its signature, without needing to look up anything in the database. An access token can be stored anywhere, such as in the client’s local storage, a header, or a query parameter.

### Access Token Advantages over session cookies
Some advantages of using access tokens over session cookies are:

- They are more scalable, as the server does not need to store or retrieve session files for each user.
- They are more secure, as they cannot be tampered with or forged by anyone who does not have the server’s secret key.
- They are more flexible, as they can be used for different types of authentication, such as OAuth or JWT.

### Notes
Cookies are used widely to save browsing session data. It can be shared amongst sites to provide user a relative experience.
For example, we add an item in cart in Amazon, but then we close the Amazon site and open facebook, only to be greeted by ads related to that item we added in cart.

Session is a period of time when a browser interacts with a server.
In nodeJS we have passport (a node module) for implementing all these.
In code, always immport express-session first.
1) express-session (middleware)
2) passport (middleware)
3) passport-local-mongoose (plugin)
4) create local strategy (username & password login) + serialize (store info into cookies) + deserialize (retrieve cookie info)

By using passport-local, we can write custom auth local strategies.

For example, visit my [sample code](https://github.com/YashNarK/node-app-template)

## Level 6: OAuth

We will ask social media apps to provide information about our users, provided the user agrees and authorize the data being shared.
Also, by using this strategy we can delegate the user credential management to the other big third party companies and get benefitted by only handling tokens.

### OAuth features
1) Granular Access Levels
2) Read / Read and Write Access
3) Revoke Access

### Steps to setup
1) Setup an app in the third party OAuth provider site.
2) Redirect user to that site for authentication while login/registration.
3) User logs in.
4) user grants permissions.
5) Third party site releases authorization code which we store in our DB
6) (optional) Exchange Auth Code for Access token. This allows our app to get subsequent info about user for some period of time. This has a little longer validity.
Auth Code => one time admit
Access Token => A pass that can be used for a period of time

For example, visit my [sample code](https://github.com/YashNarK/node-app-template)

## Example codes
- [Naren's Node JS template with user auth](https://github.com/YashNarK/node-app-template) 

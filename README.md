# express-oauth2-demo
Full OAuth2 demo with express 4 framework (http://expressjs.com/) and oauth2-server (https://github.com/oauthjs/node-oauth2-server) for nodejs.

# Background
I want to create a simple API service for my website, but some data update APIs need auth, I looked for oauth2 lite solution, but all of demos are not good. So I create one base on oauth2-server 2.4 (https://www.npmjs.com/package/node-oauth2-server) (seems 3.0 has lots of problems, at least for me).

# Steps
## 1. Install all packages
After clone the code, run:
```
npm install
```
## 2. Check models
I create 4 models for this demo, MySql, Microsoft SQL Server, Data in Memory and JWT, by default it will use memory model, and except JWT model other 3 models' token are ramdomly.

### Memory Model
**File path:** ```modules/memoryModel.js```  
**Description:** By default this demo use this model, so you don't need to change any code, you can run the demo (in follow steps), for this model, all data store in memory, if you restart the site, all things will disappare.
### JWT Model
**File path:** ```modules/jwtModel.js```  
**Description:** This model could help you integrate Json Web Token (JWT, for more JWT related information please visit: https://jwt.io/), because the client and user should be integrated with a type of database, so in this demo it use MySql, so please run the ```modules/mysql.sql``` script to create related database, tables and data, and go to ```modules/sqlPool.js``` to change your database connection information in ```sqlPool```, after that change one line code in file: ```modules/oauth.js```, replace ```memoryModel``` to ```jwtModel```, and go to ```app.js``` to change JWT secret: ```global.jwtSecret```, and JWT expired time: ```global.jwtExpiresIn```, after that you can run it.
### MS SQL Server Model
**File path:** ```modules/mssqlModel.js```  
**Description:** Because I use Microsoft Azure, so I use SQL Server Database on Azure, the data table creation sql file at: ```modules/mssql.sql```, just 3 tables need, after this, you can change one line code in file: ```modules/oauth.js```, replace ```memoryModel``` to ```mssqlModel```, and go to ```app.js``` to change your database connection information in ```global.conn```, after that, creaete data in client table and user table, after that all things work as the memory model, you can run it.
### MySQL Model
**File path:** ```modules/mysqlModel.js```  
**Description:** I use a self-deploy MySQL Database, the data table creation sql file at: ```modules/mysql.sql```, database, 3 tables and two data created, after this, you can change one line code in file: ```modules/oauth.js```, replace ```memoryModel``` to ```mysqlModel```, and go to ```modules/sqlPool.js``` to change your database connection information in ```sqlPool```, after that you can run it.

## 3. Start
After you follow the step 2 to change model that you want, you can run:
```
npm start
```
Visit: http://127.0.0.1:3000, you can see the default home page of express 4.
## 4. Check information
The first step of oauth2 is make sure you have client and user, if you are using memory model you can go to the file: ```modules/memoryModel.js``` to check the client and user information, by default:
```
clientId: futebol
clientSecret: ronaldobrilhamuito
username: esportudo
password: tudodoesporte
```
If you are using mssql model, you can create your client and user information in you database tables.
## 5. Get Token
After your client and user information ready, use Postman to send below request:
```
POST /auth/token HTTP/1.1
Host: 127.0.0.1:3000
Content-Type: application/x-www-form-urlencoded
Cache-Control: no-cache

grant_type=password&client_id=futebol&client_secret=ronaldobrilhamuito&username=iwilsesportudoonwu&password=tudodoesporte
```
Response:
By default the ramdom token:
```js
{
    "token_type": "bearer",
    "access_token": "16848b43898dba304f33d78a6f9671ddf96d9c04",
    "expires_in": 1209600,
    "refresh_token": "ac3b257a3459ac0a92f905d20a61593fdccdb151"
}
```
Or for JWT:
```js
{
    "token_type": "bearer",
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJjbGllbnRJZCI6Im5hbWVhcGkiLCJ1c2VySWQiOjEsImlhdCI6MTU0OTk4ODM0NywiZXhwIjoxNTUxMjg0MzQ3fQ.aBLwpH0SEwk3HaVuWb_bDxx9nvpknpghH5jHyrTNkVA",
    "expires_in": 1209600,
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJjbGllbnRJZCI6Im5hbWVhcGkiLCJ1c2VySWQiOjEsImlhdCI6MTU0OTk4ODM0NywiZXhwIjoxNTUxMjg0MzQ3fQ.aBLwpH0SEwk3HaVuWb_bDxx9nvpknpghH5jHyrTNkVA"
}
```
Now you get the token, then run:
```
GET /users/profile/ HTTP/1.1
Host: 127.0.0.1:3000
Authorization: Bearer 16848b43898dba304f33d78a6f9671ddf96d9c04
Cache-Control: no-cache
```
Response:
```
Wilson Wu is a Software Architect!
```
If you use wrong token:
```
GET /users/profile/ HTTP/1.1
Host: 127.0.0.1:3000
Authorization: Bearer xxxxxx
Cache-Control: no-cache
```
Response:
```js
{
    "code": 401,
    "error": "invalid_token",
    "error_description": "The access token provided is invalid."
}
```
# All things done! Now you can follow the ```/auth/token``` and ```/users/profile``` code to extend your site to support oauth2
# Enjoy your Express 4 + OAuth2 demo!

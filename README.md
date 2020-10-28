# Reverse Engineering Code

## Description

This web application allows the user to sign-up using an email account and a personal password to create an account within the database. After the user signs up for an account, they can access this account with the same email and password. This email and password is stored in a database which keeps a record of the user's account confidentially. 

## Tutorial

This tutorial will provide the neccessary information about how this application works, what each file is doing/requiring/exporting, and why they are important to the functionality of the application.

### Table of Contents
1. [Front-End](###Front-End)
2. [Routes](###Routes)
3. [Models](###Models)
4. [Config](###Config)
5. [Changes](###Changes)

#### Front-End 
 The front-end and public file for this application starts with a simple homepage which asks the user to sign-up for an account using an email and password as shown in the image below.\
![signup](/images/signup.png) \
After signing up for an account with an email and password, you can login to the same account using the same email and password used to sign up. These data are stored within a secure database. The login screen is shown in the image below.\
![login](/images/login.png)\
After logging into the account, you will be redirected to a members page which will display the member of the account. All of these front-end pages are found within the public folder named after each function of the page, *login.html*, _members.html_, and *signup.html*.

##### Front-End-JavaScript
Each front-end html page includes a front-end javascript side for the form to submit into the database. These files can be found within the public folder under js. The file _signup.js_ validates if the inputs entered by the user is acceptable and sends a post request to the server and redirects the user to the members page as shown in the code below.
```function signUpUser(email, password) {
    $.post("/api/signup", {
      email: email,
      password: password
    })
      .then(function(data) {
        window.location.replace("/members");
      })
      .catch(handleLoginErr);
  }
```
The file _login.js_ and *members.js* pretty much does the same also. The only difference is _members.js_ does a get from the database to display the correct member depending on the account info used to sign in.

#### Routes
For the front-end to know which page to display, they request from the server **routes** which will navigate the user to the correct page. These routes are located in the routes folder of this app. Within this folder, there are two route files; one which is _html-routes_ and the other _api-routes_. *html-routes* displays the html page to the user, for example it directs the user to the login or signup page depending on which they are navigating. The second routes listens to posts requests the user makes whether they create an account or login and directs to the corresponding page. 

#### Models
In order for _api-routes_ to handle the requests made by the user, it _requires_ the **Models** files to create the user's account. How the file does this is within the _user.js_ file, in which the code defines the email and password and if the input is valid, returns the user's info. The index.js file will then use the info and create elements to add into the database.

#### Config
This config folder contains a _passport.js_ file which requires the models to fetch the user's account info. This file will find the corresponding member when the user is logging into the account they created as shown in the code below.
```function(email, password, done) {
    db.User.findOne({
      where: {
        email: email
      }
    }).then(function(dbUser) {
      if (!dbUser) {
        return done(null, false, {
          message: "Incorrect email."
        });
      }
      else if (!dbUser.validPassword(password)) {
        return done(null, false, {
          message: "Incorrect password."
        });
      }
      return done(null, dbUser);
    });
```
Along with this passport.js file, there is a config.json which is the file we use to connect to the database where the data is stored by entering the correct credentials to access the database. In order to provide security for the data in this database, a _isAuthenticated.js_ file is used as middleware to allow access to a member page only if the user submits the correct credentials.

#### Changes
With this tutorial, a developer can add this application to their own work if they desired. Following this tutorial shows where the user is being directed to based on what the user is looking for; one can even add more routes if desired. A developer would be able to change email to create a username for the signup of an account with this tutorial too by changing the elements inside the models folder, but also making sure any corresponding elements be changed to match the correct element.

## Made With
- HTML
- CSS 
- JavaScript
- Node.js
- Express
- mySql

## Credits
**Jerry Leong**
jerry.leong20@gmail.com

## License
Copyright 2020 Jerry Leong

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
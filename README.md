# Reverse Engineering Code

## Description

This web application allows the user to sign-up using an email account and a personal password to create an account within the database. After the user signs up for an account, they can access this account with the same email and password. This email and password is stored in a database which keeps a record of the user's account confidentially. 

## Tutorial

This tutorial will provide the neccessary information about how this application works, what each file is doing/requiring/exporting, and why they are important to the functionality of the application.

### Table of Contents
1. [Front-End](###Front-End)
2. [Routes](###Routes)
3. [Models](###Models)

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
In order for _api-routes_ to handle the requests made by the user, it requires the **Models** files to 
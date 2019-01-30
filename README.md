# Heroku Deployment

## Lesson Objectives

1. Get started with Heroku
1. Create app on heroku
1. Attach mongolab addon
1. Update code for heroku & mongolab
1. Push git to heroku

## Get started with Heroku

- [Sign up for Heroku](https://signup.heroku.com/)
	- You may need a CC at some point although you will not be charged
- [Install Heroku CLI Tools](https://toolbelt.heroku.com/)
	- Verify by typing `heroku login` anywhere in your terminal
	- Follow prompts to sync up your heroku credentials, DO NOT HEROKU CREATE yet.

## Create an app on heroku

- Once you have Heroku CLI, you can access terminal commands to heroku.
- Let's start by creating an app on heroku. Don't rush this if you want to customize a name.
	- `heroku create [unique name]` from your project's root directory where you first initialized git.
	This will check heroku to see if the app name exists, if so you'll get an error message and have to try again.
	- If you don't specify a name, heroku will generate a unique name for you. There names are pretty cool and somewhat thematic so feel free to do either.
	- You can also do this step off their website if you want but since you'll be working in terminal anyway, might as well just do it through terminal.
	- Notice that if you successfully created a heroku app, you can see that the heroku remote was automatically added to your project's repo. Confirm this by typing `git remote`, you should see `origin` as well as `heroku`.

## Attach mongolab addon

Now that you've partitioned an app on heroku's side, you need to attach a free addon called mongolab. Mongolab provides you with heroku's version of mongodb. Up until now, we've just been using express on local connecting to our local mongodb. Now we need to connect our heroku app onto heroku's version of mongodb.
- Go to heroku and login, then hit personal apps (https://dashboard.heroku.com/apps), click on your new app, then click on the resources tab.
- Search for mLab MongoDB and add the free version `Sandbox`.




## Update code for heroku & mongolab & Environment Variables

- We will have to set up environment variables in order to get our app to run correctly on not only are local machines, but also on the heroku servers as well.  Right now we have two different connection strings for our mongodb.  One for our local environment, and one for the mLab MongoDB lab you just added.  So we are going to use a file that will allow us to define those specific `environment` variables locally and then we will define them on heroku.

1.  `npm install dotenv` [dot env docs](https://www.npmjs.com/package/dotenv)
2. At the top of your server file put the line `require('dotenv').config()`, this will make our environment variables accessible throughout our entire application.
3. Now we will create our environment variables that are local to our application.  This could be our db connection string, port number, api keys, etc... So in a very simple case we will create a file called `.env` at the root of our application

*inside of .env*
```
MONGODB_URI=mongodb://localhost/blog5
PORT=3000
```

Then update your app where ever you may be using these variables, so in the is example 

*db/db.js*

```javascript
const connectionString = process.env.MONGODB_URI;
```

*Server*
```javascript
app.listen(process.env.PORT, () => {
  console.log('listening on port 3000');
})
```

*Heroku* 
- The Port and MONGODB_URI will automatically be defined on the heroku servers for you, so locally your values for `process.env.VAR_NAME` will be equal to what you have in your `.env` file. While the values on the heroku servers will be define on heroku, you can check the values on heroku at

1.  your app name
2.  settings
3.  then click *reveal config vars* , (you should then see a variable called MONGODB_URI which should be equal to the mLab mongodb addon you did earlier.  
4.  If you need to add an api key or anything else on heroku this is where you would do it.  


Almost done, now all we need to do is make sure we set up our package.json file as well as ensure our code is abstracted to use heroku's services as well as our dev local.

- Edit your package.json file so that the "main" property is set to the name of your main application's entry point js file (probably `server.js`).

- Inside of your package.json you will also want to add a start script like the following 

```
  "scripts": {
    "start": "node server.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },

```

We also need to make sure we're ignoring the `node_modules` directory.  If you haven't already, make sure you have a .gitignore file that has in it "node_modules", (because your heroku server will install those for you)



## Push Git

- First update your remote repo so you're code is up to date.
	- `git add -A`
	- `git commit -m "updating package.json and updating code for heroku"`
	- `git push origin master`
- Now also push to heroku
	- `git push heroku master`

Wait 1 minute then type `heroku open`. You should have your deployed app open up in your browser.

- If thing's don't work out, relax and try to find out the error.
- `heroku logs`


- Note if you want to update heroku you will have to push new code up to heroku master

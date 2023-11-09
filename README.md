#  MPLS Dog Boarding Nodejs Express App - Routing + Request/Response parameters (Lab 2)
For this lab, you are going to create the backend routing and logic to be able to receive information from a contact us form.  This lab focuses on using Nodejs as the back-end server-side language and implementing third-party modules Express.js and handlebars.js).

By the end of this lab, you should able to:
1. Create a route using the Express.js framework
2. Code logic that will allow you to respond to a 'GET' request from the server.
3. Implement logic that sends data to the client to display within HTML page.

## Requirements

### Step 1 - Create a web project and implement version control and node package manager.
1.  Using your preferred editor.  Open the contents of this repository in your developement environment. **[lab2]**
2.  Make sure git is initialized and check the staus of your files.  Make sure your local branch name aligns with your remote branch name.  **[git init]** **[git status]**
3.  Create a .gitignore file and add **[node_modules]** to the file.
4.  Install NPM and complete the configuration questions to create the package.json file  **[npm init]** to the file.


### Step 2 - Installing and Configuring Express middleware
1.  Install the express.js from NPM.  command **[npm install express]**
	1.  The command adds express express.js as a dependency to your package.json file.  Open the file to confirm that express.js has been added as a dependency.  You should see express and it's version under dependencies.
    
2. Create an entry file called **index.js** 
	1.  Open the **index.js** file and make the express dependency available to your index.js file by adding the statements below to the top of your index.js file. The last statement will run your application on port 3000.
      ```javascript
	     const express = require('express')
	     const app = express()
	     const port = 3000
	
	  ```
3.  Add the middleware code below to your index.js file and review the comments above each statement to gain a better understanding of the code.
```javascript
/**To serve static files such as images, CSS files, and JavaScript files, create a folders
* and include the below statement.  The below statement assumes that I have a folder named assets in my project directory
**/
app.use(express.static('assets'))

// parse application/json -> Allow us to parse and use JSON data
app.use(express.json());

// For parsing application/x-www-form-urlencoded -> Allows us to receive form data via an 'GET' request
app.use(express.urlencoded({ extended: true }));
```
4.  Add a 'GET' route to your index.js file to test and see if application renders homepage.  Add the follow code below underneath the existing code in index.js file.
```javascript

// Route to  home
app.get('/', function (req, res) {
	
    // Use res.sendFile to send an HTML file directly
    res.sendFile(__dirname + '/home.html');

})

```
5. Add the statement below to the end of your file.  This statement that allows your application use port 3000 and outputs a message to the console indicating that your application is listening on port 3000.
```javascript
app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```
6. Run command **[node index.js]** and view application in your browser by entering URL ```localhost:3000```  
7.  After viewing application in browser kill the server by entering **Windows-users: [Control + C] - Mac-users: [CMND + C]** in the terminal.
	
### Step 3 - Adding route for contact.html page **[CHALLENGE STEP]**
1.  Underneath your home route, add a new route in the index.js file that will allow the end-user to display the contact.html page when they navigate to ``` localhost:3000/contact```
2.  Test to see if you properly implemented the route by starting server and navigating to the contact page.
   
		
### Step 4 - Adding Templating library Handlebars.js to application
1.  Install NPM package hbs as a dependency for this application by running this command -> **[npm install hbs]**

2.  Add the code below above underneath the ```app.use(express.static('assets'))``` statement
	```javascript
    // view engine setup -> We'll use handlebars.js as our templating engine
    app.set('view engine', 'html');
	// allows our application to use .html extension 
	app.engine('html', require('hbs').__express);
	```
3.  Adjust application to work with Handlebars.js
    Take sometime and familiarize yourself with handlebars.js by visiting the documentation at this [link](https://handlebarsjs.com/guide/#what-is-handlebars).  This [video](https://www.youtube.com/watch?v=hh45sR9WNH8) also does a good job with explaining the setup of handlebars.js.  
	1. Create a folder called **[views]** and add the ```home.html``` and ```contact.html``` page to the **[views]** folder.
	1. Remove this statement ```javascript res.sendFile(__dirname + 'home.html');``` found in the home route and replace with ```javascript res.render('home.html')``` .  Make this same adjustment to your contact page, but make sure the render function uses 'contact.html' instead of 'home.html'.
	2. Demonstrate passing data to ```home.html``` altering your home route ```res.render('home.html')``` by adding an object that contains one key/value pair. The res.render('home.html') statement should reflect the statement below.
	```javascript
	   // this statement sends data to the home.html page and we can use the title within our home.html page 
       res.render('home.html', {title: "MPLS #1 Dog Boarding School"})
	```
	4.  Alter the **[home.html]** page by changing the title tag to use the data passed to the the page from our route.  The title tag should reflect the statement below.
	```html
	   <title>{{title}} </title>
	```
	3. Start your server and test your application to see if your application renders the contact and home pages.  Check the browser tab to see if text displays "MPLS #1 Dog Boarding School". Stop server and proceed to Step 5.


### Step 5 - Passing Data from client to Server + Using request and response parameters.
1.  Open the **[contact.html]** page and locate the action attribute in the form and change the value to **"/submit"**. You html code should look similar to what is below.  and add the square brackets listed below.  **Be careful not to confuse user.js with users.json**
	```html
	   <form action="/submit" method="GET">
	```

2.  Create a new route in your **[index.js]** file and add the following code beneath the contact route.
	```javascript
	   app.get('/submit', (req, res) => {
          // the statement below assigns the paramters passed from the from via the name attribute to the variable formInfo.  
          var formInfo = req.query;

		  // the second argument passes data back to the 
          res.render('confirmation', {name : formInfo.fname, contact_method: formInfo.preferred_method})
       })
	```
	Insightful moment  -> If the form attribute method value was **'POST'**, then the statement to retrieve information form client would use **req.body**.  


3.  Create a new page in your views folder and name it **[confirmation.html]**. Copy the contents of the ```contact.html``` file and remove the code listed below from the confirmation.html page and add a message in its place that states **'<u>Taylor</u> someone will be contact via <u>phone</u> within 24 hours.'** where <u>Taylor</u> and <u>phone</u> is data the user entered in the form for first name and preferred method of contact sent from the submit route.    *Refer to Step 5 #3 and #4 for context. **[CHALLENGE]** 
	```html
             <h2>Touch basis with us!</h2>
                    <form action="auth.html" method="GET">
                        
                        <label>First Name: </label> <input type="text" maxlength="30" id="first" name="fname"><br>
                        <label>Last Name: </label> <input type="text" maxlength="30" id="last" name="lname"><br>
                        <label>Email: </label> <input type="text" id="email" maxlength="30"><br>
                        <label>Phone #</label> <input type="tel" id="phone"  maxlength="30"><br>
                        <label>Preferred Method of contact:</label>
                        <select name="preferred_method" id="preferred_method">
                            <option value="unselected"></option>
                            <option value="phone">Phone</option>
                            <option value="text">Text</option>
                            <option value="email">Email</option>
                        </select><br>
                        <label>Comment:</label> <br>
                        <textarea rows="4" cols="50">

                        </textarea><br>
                        <button type="submit" id="submit">Send</button>
                    </form>
	```

4.  Start Server and submit the form on the contact.html page and ensure that the form submits and renders the confirmation page along with name and preferred contact method.  Kill server and proceed to Step 6.
	
### Step 6 - Adding to confirmation.html file + Adding JS to contact form button **[ 3 Points of Extra Credit]**
1.  In addition to sending data back to the confirmation.html file, allow the values for last name, email, phone and comment to display within the confirmation message. (2 pts)
2.  Locate the button within the about us sectoin on the ```home.html``` page and add JS that would allow the end-user to navigate to the contact page once button is clicked. (1pt)
3.  Start server and test to make sure functionality works.  Kill Server by running command **[CTRL + C]** 


### STEP 7 - Submission
1.  Comment your name to the index.js file and review your application prior to pushing your work to the repository
2.  Make sure your main branch is clean and push up your final changes.
3.  In Sakai, submit the URL to your repository.  If you completed the extra credit, leave a message notifying me of your attempt.

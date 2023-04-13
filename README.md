# webdev Level 1 (Vanilla Days!)

Basics of web development

# Goal

Goal is to build a small search engine across the Metropolitan Museum of Art of New-York objects. Your application allows users to look for in the museum database with a small search form and show information and pictures of them.

As a user, you should be able to perform:

- Choose a department
- Choose if you wants only result with "On view in museum" (checkbox)
- Type a keyword in in a field (e.g. sarcophagus)
- Click to search
- Show results in a table
- Click on detail on each line
- See the details of the selected objects

As a administrator, You should to be able to:

- Show statistics about usage of your site

To acheive this, 3 steps:

1. Understand and build test API suites with POSTMAN
2. Make a static web page
3. Make this page dynamic by using JavaScript and API call based on test suite

Good luck, I will enjoy my coffee while you work hard ;)

# Steps

## Step 1 - Build your requests with POSTMAN (2h)

First approach is to prepare a little bit your data exchange and allow you to:

- Understand how the API works (could be very complex)
- Know if your are able to perform required requests on the API
- Have several samples to use when you need to call the API in a application

### Expected result

Build two collections in POSTMAN with samples requests, "Museum" and "Visits"

#### Collection "Museum"

API documentation is available at https://metmuseum.github.io/

You need 4 requests:

- Get a list of departments ID
- Search object with a keywork by department ID
- Search object with a keywork by department ID and Add criteria to get only "visible object in museum" and image only 
- Get detail of an object

#### Collection "visits"

API documentation is available at https://countapi.xyz/#format

You need 4 resquests:

- Get/Hit the number of "visit" on your home page
- Get/hit the number of "search" on your site
- Get/hit the total number of click on "detail" 
- Get info on those hits

> You will need an sitename in the countapi, use your name (e.g. cyrilportales)

### Tips & help

- Begin by the simpliest request the API can do, copy/paste given samples
- Give a id to your request to quickly identify the difficulty, or a logical order (e.g. 1 Get all products, 2 Seach product by category...)

## Step 2 - Build your web page from scratch

Now it's time to have a first static mock. It's not the last version you will build so don't spend all your energy with styling. Keep this KISS (Keep It Simple Stupid).

### Expected result

Have a static HTML home page with future components. It's a kind of mock that we can validate with the customer and make it dynamic and styled later on.

### mockup

This is a sample of page structure you can begin with

![Homepage mock](docs/img/home_mock.png)

### Tips & help

A web page is a file with HTML tags. you can also link different file:

- JavaScript for scripting, for example, a file index.js could be call like this

```HTML
<!-- Do this at the end of the body HTML tag -->
<script src="index.js"></script>
```

- CSS for styling:

```HTML
<link href="styles/main.css" rel="stylesheet">
```

> Stay focus on feature and content, not design. When your feature work, begin to make this beautiful :)

To help you, you can use this sources: 

- How to build your first page: https://www.alsacreations.com/article/lire/1374-html5-structure-globale-document.html
- How to organize a page structure: https://www.alsacreations.com/article/lire/1376-html5-section-article-nav-header-footer-aside.html
- Many examples of HTML elements to copy/paste, very useful: https://www.w3schools.com/html/ 

Need inspiration? See step2 directory in this repository

## Step 3 - Implement the search form

Now let's put peaces together. Let's begin to make things dynamic by implementing the first request in HMTL/JavaScript

### Expected result

- Make the departments list dynamic (fill select with option with javascript)
- Create the search URL to run when user click on search (addEventListener and document.querySelector)
- Run the request (provided getJsondata function in get_data.js)
- The result is displayed in the JavaScript console in a raw format

### Tips & help

- Make a JavaScript file index.js and call it in the bottom of your index.html page
- JavaScript has a function fetch() that could be useful, see link below
- console.log() allow you to display things in the web dev console of the browser

#### Get data from the server

To get data from the server, you can use fetch (this function is provided in scripts/get_data.js)

```JavaScript
    if (window.fetch) {

        console.log("Fetch is available");
    
        fetch("https://collectionapi.metmuseum.org/public/collection/v1/departments")
        .then(response => response.json())
        .then(response => function(response){

            //result is here, you can parse the json
            JSON.parse(response);
        })
        .catch(error => alert("Erreur : " + error));
    
    } else {
        // Faire quelque chose avec XMLHttpRequest?
        console.log("Fetch is disabled")
    }
```

#### Use the provided getJsonData() function

```JavaScript
//function to call an URL and push json result in a function
function getJsonData(urlGet, func){
    if (window.fetch) {

        console.log("Fetch is available");
    
        fetch(urlGet)
        .then(response => response.json())
        .then(response => func(response))
        .catch(error => alert("Erreur : " + error));
    
    } else {
        // Faire quelque chose avec XMLHttpRequest?
        console.log("Fetch is disabled")
    }
}

function testGetData(data) {
    //disploy in console of your browser, you should see an object
    //This list in the exemple has only one property "departments" so you have to use this as an array
    console.log(data);
    console.log(data.departments);
    //loop to get each result in data
    for (var eachItem of data.departments) {
        console.log(eachItem);
    }

}

getJsonData("https://collectionapi.metmuseum.org/public/collection/v1/departments", testGetData);

```
#### Fill a select box

A select in HTML is made of :

- "select" to wrap the component
- "option" to add choice inside

To create a select and fill options with JavaScript, create an select with an unic ID (here departmentId)

```HTML
    <div>
        <label for="departmentId">Department</label>
        <select id="departmentId" name="departmentId">
            <option>All</option>
            <!-- Filled by javascript -->
        </select>
    </div>
```

Then, use javascript to get this select and create option inside:

```JavaScript
function fillOptionDepartment(data) {
    //This get the list of your HTML template
    const select = document.querySelector('#departmentId');

    console.log("Load Departments list"+ data);

    //This loop on the json result
    for (const item of data.departments) {
        //this line create option HTML Object with one value of json list
        newOption = new Option(item.displayName, item.departmentId);
        //This line add the HTML option to the HTML list
        select.add(newOption,undefined);
    }
}
```

> To create data object in argument use the provided function in get_data.js


## 4 - Display result in the table

Now it's time to let the user show the result of his search. the body of the table will be now changed to be built with JavaScript

### Expected result

JavaScript use the result and for each element must create a row for the table 

### Tips & help

- Give an "id" to your table to be able to handle it with JavaScript
- JavaScript is able to build HTML element or inject HTML syntax in the page. You can choose the method you prefer (it's better to use JavaScript function to build HTML but it's harder to handle when you begin)
- Think about cleaning your table between searches

## 5 - Implement the details zone

Let's your engine to be more powerful by adding a link to disploy more details on a row.

### Expected result

- When user click on a link in the row, the site make a new HTTP request to get details on the item
- The details are used to fill the details section

### Tips & help

- JavaScript has feature to hide / disploy a HTML portion thanks to CSS, could be an improvement to disploy only when result comes.

## 6 - Add counters on your site

### Expected feature

The counters must track:

- How many times the home page was visisted
- How many search user done
- How many times user decide to show the detail (whatever it is)

### Tips & help

- Use the online free hit API: https://countapi.xyz/#format
- Reminder : Postman is your friend to discover and play without UI drawbacks

## 7 - Create a new page for statistics and put the link in the home page

### Expected feature

Anybody can access to a page stats.html. This page show the statistics of your site (visits, search, details) as a table.

### mockup

### Tips & help

Use https://countapi.xyz/#format

- The main difference with the previous step is that all requests on this page don't hit the counters, it's only info
- Prefer to use XHR/fetch or JQuery method (For JQuery, you need to declare jQuery first)

## 8 - Want more? Most visited details

Change the HTML table of hot visit by a graph.

# Resources

- Create first HTML page : https://www.alsacreations.com/article/lire/1374-html5-structure-globale-document.html
- Create a HTML form: https://developer.mozilla.org/fr/docs/Web/HTML/Element/form
- Samples of form in HTML: https://www.w3schools.com/html/html_forms.asp 
- Make request with javascript level 1: https://www.pierre-giraud.com/javascript-apprendre-coder-cours/api-fetch/
- Make request with javascript level 2: https://developer.mozilla.org/fr/docs/Web/API/Fetch_API/Using_Fetch
- Javascript, for loop: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of
- Javascript, JSON arrays (advanced): https://www.sitepoint.com/loop-through-json-response-javascript/
- Javascript, create event onclick: https://www.w3schools.com/js/js_htmldom_eventlistener.asp 
- Javascript, playground: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements
- HTML Avoid to post a form: https://stackoverflow.com/questions/2825856/html-button-to-not-submit-form
- POSTMAN: use variables: https://learning.postman.com/docs/sending-requests/variables/#using-variables 

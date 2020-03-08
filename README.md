# Web-Development

## HTML

### Forms

```
<form action="/" method="post">
  <input type="text" name="num1" placeholder="First Number">
  <input type="text" name="num2" placeholder="Second Number">
  <button type="submit" name="submit">Calculate</button>
</form>
```

- `method="post"` sends the form data (makes a POST request) to the location specified at `action="/"`

## CSS

### Display
- **block**: take up the whole width of screen
  - `<p>, <h1>, <div>, <ul>, <li>, <form>`
  - can change width and height
- **inline**: take up as much space as the content (display on same line)
  - `<span>, <img>, <a>`
  - cannot change width and height
- **inline-block**: display on same line and allow for resizing

### Position
- **static**: stick with default HTML flow
- **relative**: position element relative to how it would've been positioned if it was static
  - does not take element out of HTML flow
- **absolute**: position element relative to its parent
  - takes element out of HTML flow
- **fixed**: element stays when page is scrolled

### Centering Elements
- `text-align` does not work with elements that have a specified width
- `margin: 0 auto` or `margin: auto` centers elements with a specified width

### Font Size
- `%` and `em` are inherited (they stack on top of parent properties)
- `rem` (root em) ignores parent properties

### Stacking Order
- elements that come first in HTML document are closer to back
- children sit on top of parents
- `z-index` only works if elements are positioned (but not static)

## JavaScript

`var`
- in a function is local
- in `if/while/for` statements is global

`let`
- in a function is local
- in `if/while/for` statements is local

`const`
- in a function is local
- in `if/while/for` statements is local

## JavaScript DOM Manipulation

### Selecting HTML Elements
- `document.querySelector('selector');`  
- `document.querySelectorAll('selector');`

### Changing Text
- `document.querySelector('selector').innerHTML = 'text';`
  - can put HTML tags, e.g., `document.querySelector('selector').innerHTML = '<em>text</em>';`
- `document.querySelector('selector').textContent = 'text';`

### Changing Attributes
- `document.querySelector('selector').attributes;` (returns all the attributes)
- `document.querySelector('selector').getAttribute('attribute');`
- `document.querySelector('selector').setAttribute('attribute', 'changeTo');`

### Adding/Removing Classes
- `document.querySelector('selector').classList.add('class');`
- `document.querySelector('selector').classList.remove('class');`

### Event Listeners
- `document.querySelector('selector').addEventListener('eventType', function);`

To add event listeners to multiple items, use a for loop:

```
let buttonList = document.querySelectorAll('selector');
for (let i = 0; i < buttonList.length; i++) {  
  buttonList[i].addEventListener('eventType', function);
}
```

### Perform Action After Specified Time
- `setTimeout(function, milliseconds);`

## jQuery

### Selecting HTML Elements
- `$('selector');`
  - selects *all* elements

### Changing Text
- `$('selector').html('text');`
  - can put HTML tags, e.g., `$('selector').html = '<em>text</em>';`
- `$('selector').text('text');`

### Changing Attributes
- `$('selector').attr('attribute');`
- `$('selector').attr('attribute', 'changeTo');`

### Adding/Removing Classes
- `$('selector').hasClass('class');`
- `$('selector').addClass('class');`
- `$('selector').removeClass('class');`

### Event Listeners
- `$('selector').on('eventType', function);`
- `$('selector').click(function);`
- `$('selector').keydown(function);`

To add event listener to whole document (usually for keypresses):

```
$(document).keydown(function(event) {
// use event.key to get key that was pressed
});
```

### Adding/Removing HTML Elements
- `$('selector').before('<></>');`
  - adds element before opening tag of `'selector'`
- `$('selector').after('<></>');`
  - adds element after opening tag of `'selector'`
- `$('selector').prepend('<></>');`
  - adds element right after opening tag of `'selector'`
- `$('selector').append('<></>');`
  - adds element right before closing tag of `'selector'`
- `$('selector').remove();`

### Animations
- `$('selector').animate({opacity: 0.5});`
- `$('selector').animate({margin: "20%"});`
  - only works with properties that take numeric values

## Node/Express

### Creating Server

```
const app = express();

app.get('/', function(req, res) {
  res.send('Hello');
});

app.listen(3000, function() {
  console.log('Server is running on port 3000');
});
```

`app.listen()`
- listens on a port for HTTP requests that get sent to our server

`app.get('locationOfGetRequest', function)`
- allows us to specify what should happen when a browser connects to server and makes a GET request
- callback function tells server what to do when GET request is made
- `req` contains information about request that was made

### Routes

```
app.get('/about', function(req, res) {
  res.send('I am me');
});
```
- if browser goes to `localhost:3000/about`, then the string is displayed

### Respond to GET Requests With HTML Files

```
app.get('/', function(req, res) {
  res.sendFile(__dirname + '/index.html');
});
```
- `__dirname` is the current directory

For HTML files with CSS:
- `app.use(express.static("public"));`
- create directory called `public`
- put CSS file in `public`

### Processing POST Requests

`const bodyParser = require('body-parser');`  
`app.use(bodyParser.urlencoded({extended: true}));`
- use `body-parser` to extract information from POST requests

```
app.post('/', function(req, res) {
  var num1 = Number(req.body.num1);
  var num2 = Number(req.body.num2);
  var result = num1 + num2;
  res.send('The result of the calculation is ' + result);
});
```
- `num1` comes from `<input type="text" name="num1" placeholder="First Number">`

### Making GET Requests to External Server
`const https = require('https');`
- `https` is a built-in module

```
app.get('/', function(req, res) {
  https.get('url', function(response) {
    response.on('data', function(data) {
      const weatherData = JSON.parse(data);
      const temp = weatherData.main.temp;
      const weatherDescription = weatherData.weather[0].description;
      res.write('<h1>The temperature in London is ' + temp + ' degrees Fahrenheit</h1>');
      res.write('<p>The weather is currently ' + weatherDescription + '</p>');
      res.send();
    });
  });
});
```
- can only have 1 `res.send()` in an `app.get()`
  - use `res.write()` to send multiple lines
  
### Making POST Requests to External Server
  
```
const options = {
  method: 'POST'
};
const request = https.request(url, options, function(response) {});
request.write(jsonData);
request.end();
```
  
### Using a Button to Redirect to Different Route
```
<form action="/failure" method="post">
  <button class="btn btn-lg btn-warning" type="submit">Try Again</button>
</form>
```
- put a button inside a form

```
app.post('/failure', function(req, res) {
  res.redirect('/');
});
```

### HTML Templates with EJS

Use for web pages each with similar content

- create `views` directory and template file with `.ejs` extension inside `views`

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>To Do List</title>
  </head>
  <body>
    <h1>It's a <%= kindOfDay %></h1>
  </body>
</html>
```

`app.set('view engine', 'ejs');`

`res.render('nameOfTemplateFile', {kindOfDay: valueToReplace});`

### HTML Layouts with EJS

Use for web pages each with different content but with same styling

```
<%- include('header') -%>
<div class="box" id="heading">
  <h1> <%= listTitle %> </h1>
</div>
```

`header.ejs` contains HTML code that is repeated for all web pages:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>To Do List</title>
    <link rel="stylesheet" href="css/styles.css">
  </head>
  <body>
```

### Route Parameters

```
app.get('/posts/:postName', function(req, res) {
  console.log(req.params.postName);
});
```
- anything after the colon is a variable
- `localhost:3000/posts/post-1`

## SQL

### Create Table

```
CREATE TABLE products (
  id INT NOT NULL,
  name STRING,
  price MONEY,
  PRIMARY KEY (id)
);
```

- `PRIMARY KEY` allows a column to uniquely identify each record in a table

| id  | name | price |
| --- | ---- | ----- |

### Insert Into Table

```
INSERT INTO products
VALUES (1, 'Pen', 1.20);
```

| id  | name | price |
| --- | ---- | ----- |
| 1   | Pen  | 1.2   |

```
INSERT INTO products (id, name)
VALUES (2, 'Pencil')
```

| id  | name   | price |
| --- | ------ | ----- |
| 1   | Pen    | 1.2   |
| 2   | Pencil |       |

### Read From Table
`SELECT name, price FROM products;`
| name   | price |
| ------ | ----- |
| Pen    | 1.2   |
| Pencil |       |

```
SELECT * FROM products
WHERE id = 1;
```

| id  | name   | price |
| --- | ------ | ----- |
| 1   | Pen    | 1.2   |

### Update Table

```
UPDATE products
SET price = 0.80
WHERE id = 2;
```

| id  | name   | price |
| --- | ------ | ----- |
| 1   | Pen    | 1.2   |
| 2   | Pencil | 0.8   |

```
ALTER TABLE products
ADD stock INT
```

| id  | name   | price | stock |
| --- | ------ | ----- | ----- |
| 1   | Pen    | 1.2   |       |
| 2   | Pencil | 0.8   |       |

### Delete From Table

```
DELETE FROM products
WHERE id = 2;
```

| id  | name   | price | stock |
| --- | ------ | ----- | ----- |
| 1   | Pen    | 1.2   |       |

### Foreign Keys

```
CREATE TABLE orders (
  id INT NOT NULL,
  order_number INT,
  customer_id INT,
  product_id INT,
  PRIMARY KEY (id),
  FOREIGN KEY (customer_id) REFERENCES customers(id),
  FOREIGN KEY (product_id) REFERENCES products(id)
);
```

- `FOREIGN KEY` links tables together
  - they refer to the `PRIMARY KEY` of the table being linked

## MongoDB

- A `collection` is like a table
- A `document` is like a row

### Create Database
`use shopDB`

### Insert Document
`db.products.insertOne({_id: 1, name: 'Pen', price: 1.20})`

### Read From Collection

#### with query
`db.products.find({name: 'Pencil'})`

#### with query operators
`db.products.find({price: {$gt: 1}})`

#### with projections
`db.products.find({_id: 1}, {name: 1})`
- `_id` is `1` (true) by default

### Update Document
`db.products.updateOne({_id: 1}, {$set: {stock: 32}})`

### Delete Document
`db.products.deleteOne({_id: 2})`

## Mongoose
`const mongoose = require('mongoose');`

- a `model` is "like" a collection

### Connect to Server
`mongoose.connect('mongodb://localhost:27017/fruitsDB', {useNewUrlParser: true, useUnifiedTopology: true});`

### Create Document

```
const fruitSchema  = new mongoose.Schema ({
  name: String,
  rating: Number,
  review: String
});

const Fruit = mongoose.model('Fruit', fruitSchema); // creates fruits collection

const fruit = new Fruit({
  name: 'Apple',
  rating: 7,
  review: 'Pretty solid as a fruit'
});

fruit.save();
```

### Inserting Multiple Documents

```
Fruit.insertMany([kiwi, orange, banana], function(err) {
  if (err) {
    console.log(err);
  }
  else {
    console.log('Successfully saved all the fruits to fruitsDB');
  }
});
```

### Read From Model

```
Fruit.find(function(err, fruits) {
  if (err) {
    console.log(err);
  }
  else {
    console.log(fruits);
  }
});
```

### Update Document

```
Fruit.updateOne({_id: '5e5f57653eae65138c257fce'}, {name: 'Peach'}, function(err) {
  if (err) {
    console.log(err);
  }
  else {
    console.log('Successfully updated the document');
  }
});
```

### Delete Document

```
Fruit.deleteOne({name: 'Peach'}, function(err) {
  if (err) {
    console.log(err);
  }
  else {
    console.log('Successfully deleted the document');
  }
});
```

### Establishing Relationships

```
const personSchema = new mongoose.Schema ({
  name: String,
  age: Number,
  favoriteFruit: fruitSchema
});

const person = new Person({
  name: 'Amy',
  age: 12,
  favoriteFruit: fruit
});
```

## Security

### Database Encryption

`const encrypt = require('mongoose-encryption');`

`userSchema.plugin(encrypt, { secret: process.env.SECRET, encryptedFields: ['password'] });`

### Environment Variables

`require('dotenv').config()`

### Hashing

`const md5 = require('md5');`

### Salting
- salting is appending a random string to a password so that the whole string is used for hashing

`const bcrypt = require('bcrypt');`

## Server Starting Code

```
const express = require('express');
const bodyParser = require('body-parser');
const ejs = require('ejs');
const mongoose = require('mongoose');

const app = express();

app.set('view engine', 'ejs');
app.use(bodyParser.urlencoded({extended: true}));
app.use(express.static('public'));

mongoose.connect('mongodb://localhost:27017/databaseDB', {useNewUrlParser: true, useUnifiedTopology: true});

app.listen(3000, function() {
  console.log('Server started on port 3000');
});
```

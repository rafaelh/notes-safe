# JavaScript Syntax

``` javascript
// There are 3 essential data types: string, number (int + float), boolean (true/false)
var myString = 'undefined';
var myNumber = 345345;
var myBoolean = false;
console.log('Name: ', myString);
console.log('Lucky Number: ', myNumber);
console.log('Good joke? ' + myBoolean + '.');


// Random Numbers
Math.random(); // returns a number between 0 and 1
Math.floor(); // rounds a number down to the nearest whole number
console.log(Math.floor(Math.random() * 100)); // generate a number between 1-100


/*
This is a multiline comment.
*/


// if statements
var stopLight = 'green';
var pedestrians = false;

if (stopLight === 'red') {
  console.log('Stop');
} else if (stopLight === 'yellow' || pedestrians === true) {
  console.log('Slow down');
} else if (stopLight === 'green' && pedestrians === false) {
  console.log('Go!');
} else {
  console.log('Caution, unknown!');
}


// switch statements

var groceryItem = 'papaya';

switch (groceryItem) {
  case 'tomato':
    console.log('Tomatoes are $0.49');
    break;
  case 'lime':
    console.log('Limes are $1.49');
    break;
  case 'papaya':
    console.log('Papayas are $1.29');
    break;
  default:
    console.log('Invalid item');
    break;
}


// Functions
function multiplyByNineFifths(celsius) {
  return celsius * (9 / 5);
}

function getFahrenheit(celsius) {
  return multiplyByNineFifths(celsius) + 32;
}

console.log('The temperature is ' + getFahrenheit(15) + '°F'); // Output: The temperature is 59°F


// Arrays
var bucketList = ['See mount everest', 'Have grandchildren', 'Learn more'];
var listItem = bucketList[0];
console.log(listItem); // Output: See mount everest

// .length shows the length of an array
var hello = 'Hello World';
console.log(hello[6]); // Output: W
console.log(hello.length); // Output: 11

// .push() adds items to an array
var bucketList = ['item 0', 'item 1', 'item 2'];
bucketList.push('item 3', 'item 4');

// .pop() removes the last item in an array
var bucketList = ['item 0', 'item 1', 'item 2'];
bucketList.pop();
console.log(bucketList); // Output: [ 'item 0', 'item 1' ]

// All array functions (such as loops): https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array


// for loops
var animals = ['Grizzly Bear', 'Sloth', 'Sea Lion'];

for (var i = 0; i < animals.length; i++) {
  console.log(animals[i]);
}

// Output:
// Grizzly Bear
// Sloth
// Sea Lion


// while Loops
var condition = true;

while (condition) {
  // code block that loops until condition is false. Not equal: !==
}


/* HTML & CSS:

Embed code in html with: <script src='js/main.js'></script>

DOM = Document Object Model. The DOM is the term for the elements in an HTML file. 
The elements are any HTML code denoted by tags like <div>, <a>, or <p>.

We can select an HTML element with JavaScript by selecting it's class attribute, like this:
var header = document.getElementsByClassName('example-class-name')

This would fine an element like this in the HTML:
<div class='example-class-name'> ... </div>

Add jQuery to an HTML file as a library:
<script src='https://code.jquery.com/jquery-3.1.0.min.js'></script>

Then in a js file:
function main() {
  var $skillset = $('.skillset');
  // Note: It is a common convention to name variables that hold jQuery 
  // selectors with a dollar sign $.
}

$(document).ready(main); 
// Checks if the page is loaded. 'main' is a callback, 
// which means that our code will wait until the document/DOM is loaded and ready. 
// jQuery calls back to the main function, therefore it is a callback.


We can hide/show elements with jQuery, like this:
$('.my-selector').hide();
$('.my-selector').show();
$('.my-selector').toggle(): // change between hide and show

or we can affect a single element with '$(this):

$('.example-class').on('click', function() {
  $(this).toggleClass('active');
});

Or do it aesthetically with:
$('.example-class').slideToggle(400);

and we can affect the following element in the DOM with .next():

function main() {
  $('.projects').hide();
  $('.projects-button').on('click', function() {
    $(this).toggleClass('active');
    $(this).next().toggle();
  });
}


We can fade in elements with:
$('.example-class').fadeIn(400); // 1 seconds = 1000 miliseconds

Make elements clickable with:
$('.example-class').on('click', function() {
  // execute the code here when .example-class is clicked.
});

We can toggle a CSS class with the following function:

$('.example-class').toggleClass('active');

Then in .css:

.active {
  background-color: #333333;
  color: whitesmoke;
}

We can replace or add text to an element in the DOM using:
$('.my-selector').text('Hello world!');

*/
```

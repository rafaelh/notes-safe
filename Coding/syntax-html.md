# HTML Syntax

```html
<!DOCTYPE html> <!-- This must be the first line of the document -->

<html> <!-- This tag should enclose all html -->

  <head> <!-- This won't be shown in the actual web page -->
    <title>My Coding Journal</title>

    <!-- Style allows CSS to be inserted. It must be in the <head> tag.
    The following CSS changes secondary headings, link colour, and makes
    images circular -->
    <style> 
    * {
        font-family: 'Georgia', 'Times', serif;
      }
    
    a {
        color: SeaGreen;
        text-decoration: none;
      }
    
    img {
          border-radius: 100%;
        }
    </style>
    <!-- Preferably, you use a link to define a style.css file, rather than using <style>
      tags -->
    <link href="/style.css" type="text/css" rel="stylesheet">
  </head>

  <body>
    <!-- Paragraphs 'p' contain text -->
    <p>Consider using the extention html5-boilerplate in VS Code</p>
    <p>This is a line<br />break.</p>

    <h1>Largest title</h1>
    <h2>Smaller</h2> <!-- CSS from above will make this arial -->
    <h3>Smaller still</h3>
    <h4>Even smaller</h4>
    <h5>Much smaller</h5>
    <h6>Smallest title</h6>
    
    <ul> <!-- Start of an 'unstructured list', ie: Bullet points -->
      <li>List item</li>
      <li>List item</li>
      <li>List item</li>
    </ul>
    
    <ol> <!-- Start of an 'ordered list', ie: a numbered list -->
      <li>List item 1</li>
      <li>List item 2</li>
      <li>List item 3</li>
    </ol>

  <a href="https://www.wikipedia.org/" target="_blank">Wikipedia</a>
  <!-- target="_blank" opens the link in a new tab -->

  <img src="https://www.example.com/picture.jpg" alt="Description of the image" />

  <!-- Combining them to link an image -->
  <a href="https://en.wikipedia.org/wiki/Opuntia" target="_blank"><img src="#" alt="A red prickly pear fruit"/></a>

  <h1 id="botswana">Botswana</h1> 
  <!-- This allows styling a particular element, and not all h1s. Then the CSS is marked up like so:
  #botswana {
    background-color: #56ABFF;
  } -->

  <h1 class="science">Scientist Discovers Important Cure</h1>
  <h1 class="science">New Study Reveals The Importance of Sleep</h1>
  <!-- Classes can be used across more than one element -->


  <!-- Elements can have multiple classes, like so: -->
  <h1 class="book domestic">The Way of the Deep</h1>
  <h1 class="book foreign">A Night in the Sky</h1>
  <style>
    .book {
      font-family: Georgia, serif;
    }

    .domestic {
      font-color: #0902CC;
    }
    
    .foreign {
    font-color: #B097DD;
    }
  </style> <!-- Style tags should only ever be in the header -->

  <div class="something">
    <!-- Encloses code for an element in the web page -->
  </div>
  <style>
    div.something {
      background-color: rgb(252, 255, 205);
      font-family: Roboto, Helvetica, sans-serif;
    }
  </style>

<img src="#" alt="A red leaf" class="leaf" />
<style>
  img.leaf {
  width: 350px; /* You can use % as well */
  height: 200px; 
  display: block; /* Allows the image to be aligned as a block level element */
} </style>

<table>
  <tr>
    <th scope="col">Header Row 1, Column 1</th> <!-- 'col' or 'row' indicates what sort of header it is -->
    <th scope="col">Header Row 2, Column 2</th>
  <tr>
    <td>Row 2, Column 1</td>
    <td>Row 2, Column 2</td>
  </tr>
  <tr>
    <td colspan=2>This merges columns</td>
    <td rowspan=2>This merges rows</td>
  </tr>
</table> <!-- <tbody>, <thead> and <tfoot> elements can be used to compartmentalize table settings, 
  which can then be styled with CSS -->

<style>
  table, th, td {
    border: 1px solid black;
    font-family: Arial, sans-serif;
    text-align: center;
}
</style>

</body>
</html>
```
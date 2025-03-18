#Web/Frontend

In this exercise, students will create an HTML page containing a parent div that spans the entire screen and a child div that is centered within it. The goal is to structure and style these elements using inline CSS and ensure that their properties and positions are correctly validated

Objective:
----------
1. HTML Structure & Styling

The parent div should:(class is parent)
    - Occupy 100% of the viewport width (100vw) and height (100vh).
    - Have a navy blue (#000080) background color.
    - Contain a text label "OUTER-DIV", placed inside a `<p>` tag.
    - The label should be positioned at the top-center of the div.
    - Use flexbox to align its content to the center.
    - Use internal CSS for all styles.

The child div should:(class is child)
    - Be centered inside the parent div.
    - Have a width of 60vw and a height of 60vh.
    - Have a deepskyblue (#00BFFF) background color.
    - Contain a text label "INNER-DIV", placed inside a `<p>` tag.
    - The label should be positioned at the top-center of the child div.
    - Use internal CSS for all styles.

## Solution:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>

    <style>
      .parent {
        height: 100vh;
        width: 100vw;
        background-color: #000080;
        display: flex;
        align-items: center;
        justify-content: center;
        
        p {
          position: absolute;
          top: 0%;
          left: 50%;
        }
      }
      
      .child {
        height: 60vh;
        width: 60vw;
        background-color: #00bfff;
        position: relative;
        
        p {
          position: absolute;
          top: 0%;
          left: 50%;
        }
      }
    </style>
  </head>
  <body>
    <div class="parent">
      <p>OUTER-DIV</p>
      <div class="child">
        <p>INNER-DIV</p>
      </div>
    </div>
  </body>
</html>
```
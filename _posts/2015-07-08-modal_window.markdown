---
layout: post
title: "JavaScript - Modal Window"
date: 2015-07-08
categories: technical javascript omaha-code-school
---
## Purpose
The purpose of the assignment was to create a modal window on a page. This was accomplished using JavaScript, CSS, and HTML.

## JS Function
Below is the function used to display and remove the modal window from the HTML view.

~~~ javascript
function overlay() {
  modal = document.getElementById("overlay");
  modal.style.visibility = (modal.style.visibility == "visible") ? "hidden": "visible";
}
~~~

The _overlay_ function finds the element in the HTML with the id "overlay". This element happens to be a div containing the information to show in the window. The _overlay_ function was applied to a link, so that when the link was clicked, the event that happened was to run the _overlay_ function.

The function itself toggles style of the selected div from visible to hidden. The default style of the modal div was hidden, so when the link is clicked on, the window becomes visible.

An additional link was included in the modal window div that also included the _overlay_ function. This "closes" the window, but toggling the visibility back to hidden.

<iframe width="560" height="315" src="https://www.youtube.com/embed/EJhn7cCYl4k" frameborder="0" allowfullscreen></iframe>

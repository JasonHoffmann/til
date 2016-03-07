#title:  Jquery extend
##date:  2016-03-07 16:39:31
##sub-folder:  Javascript

Turns out you can call a function inside of another one pretty simply using jQuery. You can even pass in arguements, as in:
`$.extend(this, originalFn.call(this, element));

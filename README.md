# Use npm package on client side

`leader-line` is a javascript libraray to darw arrows between html elements, but it requires `node` to **require** and **run**.
we want to use this in **vanilla javascript** to use it on *clients side* on browser.

another javascript library `Browserify` will be used in this technique.

It isn't enough to use Browserify. There were still issues with the client-side not finding the variables/functions from the library.  
Draw a leader line in your web page.

Here are the steps to perform:


**Install Browserify:**

```
npm install -g browserify
```


**Install a library from npm (I'll use leader-line as an example):**

```
npm i leader-line
```


You will now have all the npm files needed inside the node_modules directory:

```
node-modules
  leader-line
    bower.json
    leader-line.min.js
    LICENSE
    package.json
    README.md
```


Now you can run the usual Browserify command to bundle the JS file from the npm package:

```
browserify node_modules/leader-line/leader-line.min.js -o bundle.js
```


This will produce a bundle.js file outside of node_modules:

```
node-modules
bundle.js
package-lock.json
```


This is the file you can bring into the front-end, as you would with a usual JS library.

So, renaming bundle.js to leaderline.js, you can simply add the usual line in the header of your `index.html` file:

```html
<script src="leaderline.js" type="module"></script>
```


Notice the addition of `type="module"` to the script tag.

However, this is STILL not enough. The final step is to open the JS file for the library (in my case `leaderline.js`) and find the main function that needs to be exported (usually somewhere near the top):

```js
var LeaderLine=function(){"use strict";var te,g,y,S,_,o,t,h,f,p,a,i,l,v="leader-line"
```


you need LeaderLine to be available inside your scripts. To make this possible, you simply remove var and add `window`. in front of the function name, like this:

```js
window.LeaderLine=function(){"use strict";var te,g,y,S,_,o,t,h,f,p,a,i,l,v="leader-line"
```


Now You can use the library client-side without any problems:

**HTML**

```html
<div id="start">start</div>
<div id="end">end</div>
```


**JS**

```js
document.addEventListener("DOMContentLoaded", () => {
        const myLine = new LeaderLine(
          document.getElementById("start"),
          document.getElementById("end"),
          { dash: { animation: true } }
        );
      });
```

**Result**

<img align="center" alt="GIF" height="160px" src="demo.png" />

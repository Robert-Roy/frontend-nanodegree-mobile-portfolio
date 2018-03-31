## Website Performance Optimization portfolio project

This is a program I did for my Udacity Nanodegree. My goal was to optimize a very, very poorly optimized site. You can see the results here:
https://developers.google.com/speed/pagespeed/insights/?url=https%3A%2F%2Frobert-roy.github.io%2Ffrontend-nanodegree-mobile-portfolio%2F&tab=mobile

To view the optimized site, go here:
https://robert-roy.github.io/frontend-nanodegree-mobile-portfolio/

To download and run yourself, clone or download this repository, then open the index.html file in the browser of your choice. Click immediately on Cam's Pizza.

Optimizations:

I compressed and resized image files.

I moved CSS to in-line to prevent blocking.

Numerous javascript improvements such as:

Original:

```
var items = document.querySelectorAll('.mover');
  for (var i = 0; i < items.length; i++) {
    // document.body.scrollTop is no longer supported in Chrome.
    var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
    var phase = Math.sin((scrollTop / 1250) + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
}
```

Revised:

```
var items = document.querySelectorAll('.mover');
    // document.body.scrollTop is no longer supported in Chrome.
    var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
    var newLeft = [];
    var phases = [];
    for (var c = 0; c < 5; c++){
        phases.push(Math.sin((scrollTop / 1250) + (c % 5)));
    }
    console.log(phases);
    for (var a = 0; a < items.length; a++) {
        newLeft.push(items[a].basicLeft + 100 * phases[(a % 5)] + 'px');
    }
    for (var i = 0; i < newLeft.length; i++) {
        items[i].style.left = newLeft[i];
    }
```

Original:

```
var resizePizzas = function(size) {
  changeSliderLabel(size);

   // Returns the size difference to change a pizza element from one size to another. Called by changePizzaSlices(size).
  function determineDx (elem, size) {
    var oldWidth = elem.offsetWidth;
    var windowWidth = document.querySelector("#randomPizzas").offsetWidth;
    var oldSize = oldWidth / windowWidth;

    // Changes the slider value to a percent width
    function sizeSwitcher (size) {
      switch(size) {
        case "1":
          return 0.25;
        case "2":
          return 0.3333;
        case "3":
          return 0.5;
        default:
          console.log("bug in sizeSwitcher");
      }
    }

    var newSize = sizeSwitcher(size);
    var dx = (newSize - oldSize) * windowWidth;

    return dx;
  }

  // Iterates through pizza elements on the page and changes their widths
  function changePizzaSizes(size) {
    for (var i = 0; i < document.querySelectorAll(".randomPizzaContainer").length; i++) {
      var dx = determineDx(document.querySelectorAll(".randomPizzaContainer")[i], size);
      var newwidth = (document.querySelectorAll(".randomPizzaContainer")[i].offsetWidth + dx) + 'px';
      document.querySelectorAll(".randomPizzaContainer")[i].style.width = newwidth;
    }
  }

  changePizzaSizes(size);
};
```

Revised:

```
var resizePizzas = function(size) {
    changeSliderLabel(size);

    // Returns the size difference to change a pizza element from one size to another. Called by changePizzaSlices(size).
    function determinePizzaWidth(size) {
        // Converts the pizza size slider value to a % width for pizzas
        switch (size) {
            case "1":
                return 25 + "%";
            case "2":
                return 33.33 + "%";
            case "3":
                return 50 + "%";
            default:
                console.log("bug in sizeSwitcher");
        }
    }

    // Iterates through pizza elements on the page and changes their widths
    function changePizzaSizes(size) {
        var pizzas = document.querySelectorAll(".randomPizzaContainer");
        var newWidth = determinePizzaWidth(size);
        console.log(newWidth);
        for (var i = 0; i < pizzas.length; i++) {
            pizzas[i].style.width = newWidth;
        }
    }

    changePizzaSizes(size);
};
```
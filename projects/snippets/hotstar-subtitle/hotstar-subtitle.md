**Last updated**: September 8, 2019

```

// ==UserScript==
// @name         Change subtitle style
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  Hostar's subtitle kinda sucks... so
// @author       @lrmn
// @match        https://www.hotstar.com/*
// @icon         https://www.google.com/s2/favicons?domain=hotstar.com
// @grant        none
// ==/UserScript==

(function() {
  'use strict';
  // You can customize this
  const fontWeight = "600!important";
  const stroke = "1px black";
  const fontSize = "42px!important";
  const backgroundColor = "transparent!important";

  const styleSheet = document.createElement('style');
  styleSheet.type = "text/css"
  styleSheet.innerHTML = `
    .subtitle-container .cues-container .shaka-text-container span{
      font-size:${fontSize};
      font-weight:${fontWeight};
      background-color:${backgroundColor};
      -webkit-text-stroke:${stroke};
    }
  `

  document.head.appendChild(styleSheet)

})();

```

### How to use?

Import this script to [tampermonkey](https://www.tampermonkey.net/).

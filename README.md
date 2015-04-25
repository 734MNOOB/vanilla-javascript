# vanilla-javascript
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Selector](#selector)
- [Clone object](#clone-object)
- [Get unique values in an array](#get-unique-values-in-an-array)
- [Each loop](#each-loop)
- [Get Object length](#get-object-length)
- [Class](#class)
- [Trim](#trim)
- [Get element offset](#get-element-offset)
- [Check](#check)
- [Cookie](#cookie)
- [Flatify an array](#flatify-an-array)
- [Parse url](#parse-url)
- [Get number between $start and $end](#get-number-between-start-and-end)
- [Make an array random ordered (shuffle)](#make-an-array-random-ordered-shuffle)
- [Make HTML elements (Divs) random ordered](#make-html-elements-divs-random-ordered)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Selector

```javascript
// works like $('#id') but do $('id')
var $  = document.getElementById.bind(document);
// works like $('.class') but do $$('.class')
var $$ = document.querySelectorAll.bind(document);
```

## Clone object

```javascript
function clone(obj) {
    if(obj === null || typeof(obj) !== 'object')
        return obj;

    var temp = obj.constructor(); 

    for(var key in obj) {
        if(Object.prototype.hasOwnProperty.call(obj, key)) {
            temp[key] = clone(obj[key]);
        }
    }
    return temp;
}
```

## Get unique values in an array

```javascript
function ArrayUnique(array) {
   var u = {}, a = [];
   for(var i = 0, l = array.length; i < l; ++i){
      if(u.hasOwnProperty(array[i])) {
         continue;
      }
      a.push(array[i]);
      u[array[i]] = 1;
   }
   return a;
}
```

## Each loop

```javascript
// for an array
function each(arr, fn) {
  for(var i in arr) {
    fn(i, arr[i]);
  }
}
// for dom element, works like $.each
var elements = document.querySelectorAll(selector);
Array.prototype.forEach.call(elements, function(el, i) {
    // your action
});
```

## Get Object length

```javascript
function ObjectLength(obj) {
  var length = 0, key;
  for (key in obj) {
    if (obj.hasOwnProperty(key)) length++;
  }
  return length;
}
```

## Class

```javascript
// classList is much faster than className as we know, ref: http://t.cn/RAYkuEr
// addClass
function addClass(el, className) {
  if (el.classList)
    el.classList.add(className);
  else
    el.className += ' ' + className;
}
// removeClass
function removeClass(el, className) {
  if (el.classList)
    el.classList.remove(className);
  else
    el.className = el.className.replace(new RegExp('(^|\\b)' + className.split(' ').join('|') + '(\\b|$)', 'gi'), ' ');
}
// toggleClass
function toggleClass(el, className) {
  if (el.classList) {
    el.classList.toggle(className);
  } else {
    var classes = el.className.split(' ');
    var existingIndex = classes.indexOf(className);

    if (existingIndex >= 0)
      classes.splice(existingIndex, 1);
    else
      classes.push(className);

    el.className = classes.join(' ');
  }
}
```

## Trim

```javascript
// works like $.trim(string)
var string = ' ai shi te ru    ';
string.trim(); // print 'ai shi te ru'
```

## Get element offset

```javascript
function getOffset(el) {
  var _x = 0;
  var _y = 0;
  while (el && !isNaN(el.offsetLeft) && !isNaN(el.offsetTop)) {
    _x += el.offsetLeft - el.scrollLeft;
    _y += el.offsetTop - el.scrollTop;
    el = el.offsetParent;
  }
  return {
    top: _y,
    left: _x
  };
}
// example: var offset = getOffset(document.getElementById('yourElId'));
```

## Check

```javascript
// detect browsers
var isOpera = !!window.opera || navigator.userAgent.indexOf(' OPR/') >= 0;
    // Opera 8.0+ (UA detection to detect Blink/v8-powered Opera)
var isFirefox = typeof InstallTrigger !== 'undefined';   // Firefox 1.0+
var isSafari = Object.prototype.toString.call(window.HTMLElement).indexOf('Constructor') > 0;
    // At least Safari 3+: "[object HTMLElementConstructor]"
var isChrome = !!window.chrome && !isOpera;              // Chrome 1+
var isIE = /*@cc_on!@*/false || !!document.documentMode; // At least IE6
```

## Cookie

```javascript
// ref: https://github.com/filamentgroup/cookie
function cookie( name, value, days ){
	// if value is undefined, get the cookie value
	if( value === undefined ){
		var cookiestring = "; " + window.document.cookie;
		var cookies = cookiestring.split( "; " + name + "=" );
		if ( cookies.length === 2 ){
			return cookies.pop().split( ";" ).shift();
		}
		return null;
	}
	else {
		// if value is a false boolean, we'll treat that as a delete
		if( value === false ){
			days = -1;
		}
		var expires;
		if ( days ) {
			var date = new Date();
			date.setTime( date.getTime() + ( days * 24 * 60 * 60 * 1000 ) );
			expires = "; expires="+date.toGMTString();
		}
		else {
			expires = "";
		}
		window.document.cookie = name + "=" + value + expires + "; path=/";
	}
}
```

## Flatify an array

```javascript
var arr = [1, 2, [3, [4, 5]], 6]
var result = []
var flat = function(arr) {
  var i, l = arr.length
  for (i = 0; i < l; i++) {
    if (typeof arr[i] === 'number') {
      result.push(arr[i])
    } else {
      flat(arr[i])
    }
  }
  return result
}
arr = flat(arr) // result is [1,2,3,4,5,6]
```

## Parse url

```javascript
// parse url queries to an array, http://example.com/?q=words&ord=desc => ['words', 'desc']
var query = location.search.substring('1').split('&')
for (var i in query) {
  query[i] = query[i].split('=')[1]
}
// parse url queries to an object, http://example.com/?q=words&ord=desc => {q: 'words', ord: 'desc'}
var query = location.search.substring('1').split('&')
for (var i in query) {
  var key = query[i].split('=')[0],
    value = query[i].split('=')[1]
  query[i] = {}
  query[i][key] = value
}
```

## Get number between $start and $end

```javascript
// NumberBetween(0, 100) return a ( 0 <= number < 100 )
function NumberBetween(start, end) {
  return Math.floor(Math.random() * end) + start
}
```

## Make an array random ordered (shuffle)

```javascript
function shuffle(array) {
  var currentIndex = array.length,
    temporaryValue, randomIndex;
  // 当数组不为空时
  while (0 !== currentIndex) {
    // 随机选择
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex -= 1;
    // 和当前元素交换
    temporaryValue = array[currentIndex];
    array[currentIndex] = array[randomIndex];
    array[randomIndex] = temporaryValue;
  }
  return array;
}
```

## Make HTML elements (Divs) random ordered

```javascript
// I haven't checked this
function randomDivs(el) {
  var newDivs = []
  for (var i = 0, dom = document.querySelectorAll(el); i < dom.length; i++) {
    var randomIndex = NumberBetween(0, dom.length)
    newDivs[newDivs.length] = dom[randomIndex].cloneNode(true)
    dom[randomIndex].parentNode.removeChild(dom[randomIndex])
  }
  return newDivs
}
```

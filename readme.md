# maastr

An easy to use "custom text format parser".
Quick example to explain things:
```js
let text = 'My %custom **FORMAT**%';
let tokens = {
    '%': 'flipped',
    '**': 'bold'
};
maastr.parse(text, tokens);

// Returns:
"My <span class="flipped">custom <span class="bold">FORMAT</span></span>"
```

You can define your own tokens and parse your custom string formatting, ready to be styled in css.

## Getting Started

### Importing the lib

For using it in the browser, either use Github as your CDN and import it as the following:
```html
<script src="https://raw.githubusercontent.com/Maanex/maastr/master/latest/maastr.min.js"></script>
```

Or choose your version above and download the required files.

An npm module has not yet been created. You can download the files though to use it server-sided.

### Implementing a custom parser

Everything this api has to offer is bundled into the `maastr` object.
To parse a string, you can use the following method:
```js
maastr.parse(string, tokens?, settings?); // returns: string
```

`tokens` needs to be an object with your tokens as keys and the output classname as values. Example:
```js
{
    '**': 'bold',
    '__': 'underline',
    '~~': 'strikethrough',
    '*': 'italic'
}
```
**The maximum token length for now is limited to two!**

`settings` can be left out for now. For more information, have a look below:

## Advanced

### Settings

The `maastr.parse(...)` method takes in an optional third parameter labeled settings. A settings object looks like this, each parameter is optional and can be left out:
```js
{
    htmlTag: 'span', // The HTML tag of the output. <span> is default but if you need a <div> for example, here's where to change that
    prefix: '', // Adds text infront of the output. Can be used to wrap the whole generated HTML into another block to target styling more precisely
    suffix: '', // Same as prefix but after the output
}
``` 

### Default Tokens

If you use the same custom formatting everywhere in your project, it can be frustrating to always provide the `parse` method with the tokens. To make things easier, you can set a default `tokens` object using:
```js
maastr.default.tokens = { ... }
```

### Default Settings

Just as you can change the default tokens, you can also change the default settings. Here's how:
```js
maastr.default.settings = { ... }
```

### Single Character Wrapping

Sometimes you want to style each character individually or want to target single letters in javascript. Therefore you can replace the tagname in your token list with an array. The first entry has to be the tag name, the second one needs to be set to true in order to let maastr wrap each character into it's own html element.

Example:
```js
let tokens = {
    '$': [ 'dollar', true ],
    '%': [ 'percent', false ], // same as '%': 'percent',
    '+': [ 'plus', true ]
}

// Note: The example return values below are formatted to be better understandable. The actual output is minified!

maastr.parse('$example$', tokens);
/* returns:
 * "<span class="dollar">e</span>
 *  <span class="dollar">x</span>
 *  <span class="dollar">a</span>
 *  <span class="dollar">m</span>
 *  <span class="dollar">p</span>
 *  <span class="dollar">l</span>
 *  <span class="dollar">e</span>"
 */

maastr.parse('% more $examples$%', tokens);
/* returns:
 * "<span class="percent">
 *    more
 *    <span class="dollar">e</span>
 *    <span class="dollar">x</span>
 *    <span class="dollar">a</span>
 *    <span class="dollar">m</span>
 *    <span class="dollar">p</span>
 *    <span class="dollar">l</span>
 *    <span class="dollar">e</span>
 *    <span class="dollar">s</span>
 *  </span>"
 */

maastr.parse('+complex$example$+', tokens);
/* returns:
 * "<span class="plus">c</span>
 *  <span class="plus">o</span>
 *  <span class="plus">m</span>
 *  <span class="plus">p</span>
 *  <span class="plus">l</span>
 *  <span class="plus">e</span>
 *  <span class="plus">x</span>
 *  <span class="dollar plus">e</span>
 *  <span class="dollar plus">x</span>
 *  <span class="dollar plus">a</span>
 *  <span class="dollar plus">m</span>
 *  <span class="dollar plus">p</span>
 *  <span class="dollar plus">l</span>
 *  <span class="dollar plus">e</span>"
 */

maastr.parse('+with space+', tokens);
/* since an empty tag cannot be styled, each space in a single-character-wrapping will be replaced by the No-Break-Space &nbsp;
 * returns:
 * "<span class="plus">w</span>
 *  <span class="plus">i</span>
 *  <span class="plus">t</span>
 *  <span class="plus">h</span>
 *  <span class="plus">&nbsp;</span>
 *  <span class="plus">s</span>
 *  <span class="plus">p</span>
 *  <span class="plus">a</span>
 *  <span class="plus">c</span>
 *  <span class="plus">e</span>"
 */ 

```

---

You've reached the bottom.

[Find me on Twitter](https://twitter.com/Maanex_)

And thanks to [Egor](https://twitter.com/egordorichev) for making me turn this into a standalone lib.
# [PEG.js](https://github.com/pegjs/pegjs) Require Extension

A simple require extension for PEG.js that supports importing syntax  
  
*If you're using webpack, it's recommended to use [pegjs-import-loader](https://github.com/phuongduyphan/pegjs-import-loader)*

## Install
`npm install --save-dev pegjs-require-import`

## Usage
### Importing Syntax
*parser.pegjs:*
```js
{
    const str = 'This is just an example string';
    function concat(a, b) {
        return a.concat(b);
    }
}
Expression
  = head:Term tail:(_ ("+" / "-") _ Term)* {
      return tail.reduce(function(result, element) {
        if (element[1] === "+") { return result + element[3]; }
        if (element[1] === "-") { return result - element[3]; }
      }, head);
    }
    
Factor
  = "(" _ expr:Expression _ ")" { return expr; }
  / Integer
 
@import './base-rules.pegjs'
@import './keywords.pegjs'
```
*Import syntax is the same as in [pegjs-import-loader](https://github.com/phuongduyphan/pegjs-import-loader)*  
  

### Generate a parser in JS code using require
```js
const pegjs_require = require('pegjs-require-import');
const parser = pegjs_require('./parser.pegjs', {
  format: 'commonjs',
  dependencies: {
    _: 'lodash'
  }
});
const result = parser.parse(content);
```
## API
### pegjs_require(file_path, options)
#### options
Type: object  
[See more about PEG.js options](https://pegjs.org/documentation)

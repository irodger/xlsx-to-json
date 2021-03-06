# xlsx-json-parser
[![Build Status](https://travis-ci.org/irodger/xlsx-json-parser.svg?branch=master)](https://travis-ci.org/irodger/xlsx-json-parser)
[![NPM version](https://badge.fury.io/js/xlsx-json-parser.svg)](http://badge.fury.io/js/xlsx-json-parser)
[![Downloads](https://img.shields.io/npm/dm/xlsx-json-parser.svg)](http://npm-stat.com/charts.html?package=xlsx-json-parser)
[![License](https://img.shields.io/github/license/irodger/xlsx-json-parser.svg?style=flat-square)](https://github.com/irodger/xlsx-json-parser/blob/master/LICENSE)
[![Issues](https://img.shields.io/github/issues/irodger/xlsx-json-parser.svg?style=flat-square)](https://github.com/irodger/xlsx-json-parser/issues)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](https://github.com/irodger/xlsx-json-parser/pulls)
[![dependencies status](https://img.shields.io/david/irodger/xlsx-json-parser.svg?style=flat)](https://david-dm.org/irodger/xlsx-json-parser)
[![devDependencies status](https://img.shields.io/david/dev/irodger/xlsx-json-parser.svg?style=flat)](https://david-dm.org/irodger/xlsx-json-parser#info=devDependencies)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/3ff1ed296833401497b52b15e4ceb9d9)](https://www.codacy.com/app/irodger/xlsx-json-parser)

## Description and nuances
xlsx to JSON parser. Also support i18next format. Relies on node-xlsx.   
### Yep, it can append new files to exist now! *Cheers* 😎

### What's new
* v.0.3.5 - Update Readme.md
* v.0.3.4 - Improve recursiveSearchValues & update tests
* v.0.3.3 - Update Readme.md
* v.0.3.2 - Fix known issues with one level template
* v.0.3.1 - Update Readme.md
* v.0.3.0 - Add config tests. Fix parser. Now it's work as NPM module correctly
* v.0.2.5 - Codystyle fixes
* v.0.2.4 - Remove async write file & some refactoring
* v.0.2.3 - Add author's contacts
* v.0.2.2 - Add jest testing
* v.0.2.1 - Refactoring
* v.0.2.0 - Add append to exist files
* v.0.1.0 - First init

### Install
```javascript
npm i xlsx-json-parser
```
Then add folder `xlsx` (you can change it in xlsx-json-parser.config.js) in your project folder

### Usage
#### In command line
```javascript
node node_modules/xlsx-json-parser
```

#### You can add this line to your `package.json` in scripts section
```json
"scripts": {
    "xlsx-parse": "node node_modules/xlsx-json-parser"
  }
```
And when you need to parse xlsx type in your console `npm run xlsx-parse`

#### Using with arguments
```javascript static
node node_modules/xlsx-json-parser --ln=listNumber --xlsx=xlsxDir --json=jsonDir --wfn
```
##### Where
`--ln` - List number  
`--xlsx` - Path to xlxs
`--json` - Path to json  
`--wfn` - With file names for json files   
You can change default arguments in `xlsx-json-parser.config.js`

#### Config file
Config file `xlsx-json-parser.config.js` looks like
```javascript
const config = {
  xlsxDir: 'xlsx',
  jsonDir: 'json',
  listNumber: 1,
  withFilenames: false,
  fileTemplate(lang, array) {
    return `{
  "${lang}": {
    "translations": ${JSON.stringify(array)}
  }
}`;
  },
};

module.exports = config;
```
You can overwrite it by put `xlsx-json-parser.config.js` to your directory

##### Where
`xlsxDir` - default folder with xlsx files  
`jsonDir` - default folder for json files  
`listNumber` - Number of needed list in Excel file  
`withFilenames` - Add to json files name of xlsx files if false, json files will be overwritten
`fileTemplate` - method for set how json might looks, where `lang` - is current language for file, `array` - items for this langugae   

### i18next Format
By default keys for all langs parser get value from first lang
```json
{
  "lang": {
    "translations": {
      "key": "value",
      "another_key": "another value"
    }
  }
}
```
You can change format in `xlsx-json-parser.config.js`

### Files
Parser looks into `xlsxDir` and parse all xls/xlsx files.  
Return filenames looks like `{lang}_{xlsxFileName}.json`

### Xlsx format
To correct parsing you need next format - First row in xlsx file is for languages. Rows are for keys and values.

### Append to exist files
Yep! It can be append to exist files if your JSON file format in config equal to your exist JSON view.

### Templates
If you write next template 
```javascript
fileTemplate(lang, array) {
    return `{
  "${lang}":  ${JSON.stringify(array)}
}`;
    }
```
You recieve
```json
{
  "lang": {
      "key": "value",
      "another_key": "another value"
  }
}
```

If you write next template 
```javascript
fileTemplate(lang, array) {
    return `${JSON.stringify(array)}`;
    }
```
You recieve
```json
{
  "key": "value",
  "another_key": "another value"
}
```
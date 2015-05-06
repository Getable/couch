# couch [![NPM version][npm-image]][npm-url] [![Build Status][travis-image]][travis-url] [![Dependency Status][daviddm-url]][daviddm-image]

Stupid simple Couch wrapper based on Request

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [Install](#install)
- [Usage](#usage)
- [Developing](#developing)
  - [Requirements](#requirements)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Install

```
npm install couch
```

## Usage

```javascript
var couch = require('couch')
  , c = couch('http://me.iriscouch.com/db')
  ;

c.post({'msg':'new document'}, function (e, info) {
  if (e) throw e
  c.post({'msg':'new document', _id:info.id, _rev:info.rev}, function (e, info) {
    if (e) throw e
    c.get(info.id, function (e, doc) {
      if (e) throw e
      console.log(doc) // {'msg':'new document', _id:<id>, _rev:<rev>}
    })
  })
})
```

## Couch

* new Couch(options) - return value from require('couch')(url)
* Couch.get(id, cb) - get a document of the specified id
* Couch.post(doc, cb) - write a document. MUST have _id and _rev if already exists
* Couch.update(id, mutate, cb) - updated an existing document atomically (regardless of revision)

```javascript
c.update('myid', function (doc) {doc.status = 'complete'}, function (e, info) {
 if (e) throw e
 console.log(info) // {seq:<seq>, id:<id>, rev:<rev>}
})
```
## Views

* Couch.all(opts, cb) - Hits the /db/\_all_docs API which accepts similar arguments and has a simpilar return value to views but is an index of all documents in CouchDB.

```javascript
c.all({keys:['onlykey1', 'onlykey2']}, function (e, results) {
  if (e) throw e
  console.log(results.rows) // [{id:onlykey1, rev:<rev>}, {id:onlykey2, rev:<rev>}]
})
```

* Couch.design(name).view(name).query(opts)

```javascript
c.design('app').view('byProperty').query({key:'type', include_docs:true}, function (e, results) {
  console.log(results.rows)
})
```

## Tests
Tests are in [tape](https://github.com/substack/tape).


* `npm test` will run the tests
* `npm run tdd` will run the tests on every file change.


## Developing
To publish, run `npm run release -- [{patch,minor,major}]`

_NOTE: you might need to `sudo ln -s /usr/local/bin/node /usr/bin/node` to ensure node is in your path for the git hooks to work_

### Requirements
* **npm > 2.0.0** So that passing args to a npm script will work. `npm i -g npm`
* **git > 1.8.3** So that `git push --follow-tags` will work. `brew install git`

## License

Artistic 2.0 © Mikeal Rogers


[npm-url]: https://npmjs.org/package/@getable/couch
[npm-image]: https://badge.fury.io/js/@getable/couch.svg
[travis-url]: https://travis-ci.org/Getable/@getable/couch
[travis-image]: https://travis-ci.org/Getable/@getable/couch.svg?branch=master
[daviddm-url]: https://david-dm.org/Getable/@getable/couch.svg?theme=shields.io
[daviddm-image]: https://david-dm.org/Getable/@getable/couch

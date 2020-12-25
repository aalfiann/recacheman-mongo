> This is a fork version of [cacheman-mongo](https://github.com/cayasso/cacheman-mongo) with following differences :
- Minimum NodeJS 10
- Removed old libraries
- Fixing all vulnerables
- Up to date
- Support for MongoDB v2 only

# recacheman-mongo

[![NPM](https://nodei.co/npm/recacheman-mongo.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/recacheman-mongo/)  
  
[![npm version](https://img.shields.io/npm/v/recacheman-mongo.svg?style=flat-square)](https://www.npmjs.org/package/recacheman-mongo)
[![Build Status](https://travis-ci.com/aalfiann/recacheman-mongo.svg?branch=master)](https://travis-ci.com/aalfiann/recacheman-mongo)
[![Coverage Status](https://coveralls.io/repos/github/aalfiann/recacheman-mongo/badge.svg?branch=master)](https://coveralls.io/github/aalfiann/recacheman-mongo?branch=master)
[![Known Vulnerabilities](https://snyk.io//test/github/aalfiann/recacheman-mongo/badge.svg?targetFile=package.json)](https://snyk.io//test/github/aalfiann/recacheman-mongo?targetFile=package.json)
![License](https://img.shields.io/npm/l/recacheman-mongo)
![NPM download/month](https://img.shields.io/npm/dm/recacheman-mongo.svg)
![NPM download total](https://img.shields.io/npm/dt/recacheman-mongo.svg)  

MongoDB standalone caching library for Node.JS and also cache engine for [recacheman](https://github.com/aalfiann/recacheman).

## Instalation

``` bash
$ npm install recacheman-mongo
```

## Usage

```javascript
var CachemanMongo = require('recacheman-mongo');
var cache = new CachemanMongo();

// set the value
cache.set('my key', { foo: 'bar' }, function (error) {

  if (error) throw error;

  // get the value
  cache.get('my key', function (error, value) {

    if (error) throw error;

    console.log(value); //-> {foo:"bar"}

    // delete entry
    cache.del('my key', function (error){
      
      if (error) throw error;

      console.log('value deleted');
    });

  });
});
```

## API

### CachemanMongo([options])

Create `cacheman-mongo` instance. `options` are mongo valid options including `port`, `host`, `database` and `collection`.

```javascript
var options = { 
  port: 9999,
  host: '127.0.0.1',
  username: 'beto',
  password: 'my-p@ssw0rd'
  database: 'my-cache-db',
  collection: 'my-collection',
  compression: false
};

var cache = new CachemanMongo(options);
```

You can also pass a valid mongodb connection string as first arguments like this:

```javascript
var options = {
  collection: 'account'
};

var cache = new CachemanMongo('mongodb://127.0.0.1:27017/blog', options);
```

Or pass a mongodb db instance directly as client:

```javascript
MongoClient.connect('mongodb://127.0.0.1:27017/blog', function (err, db) {
  var cache = new CachemanMongo(db, { collection: 'account' });

  // or
  cache = new CachemanMongo({ client: db, collection: 'account' });
});
```

#### Cache Value Compression

MongoDB has a 16MB document size limit, and, currently, does not have built in support for compression. You can enable cache value compression for large Buffers by setting the `compression` options to `true`, this will use the native node `zlib` module to compress with `gzip`. It only compresses Buffers because of the complexity in correctly decompressing and deserializing the variety of data structures and string encodings.

Thanks to @Jared314 for adding this [feature](https://github.com/cayasso/cacheman-mongo/pull/2).

```javascript
var cache = new Cache({ compression: true });
cache.set('test1', Buffer.from("something big"), function (err) {...});

```
### cache.set(key, value, [ttl, [fn]])

Stores or updates a value.

```javascript
cache.set('foo', { a: 'bar' }, function (err, value) {
  if (err) throw err;
  console.log(value); //-> {a:'bar'}
});
```

Or add a TTL(Time To Live) in seconds like this:

```javascript
// key will expire in 60 seconds
cache.set('foo', { a: 'bar' }, 60, function (err, value) {
  if (err) throw err;
  console.log(value); //-> {a:'bar'}
});
```

### cache.get(key, fn)

Retreives a value for a given key, if there is no value for the given key a null value will be returned.

```javascript
cache.get('foo', function (err, value) {
  if (err) throw err;
  console.log(value);
});
```

### cache.del(key, [fn])

Deletes a key out of the cache.

```javascript
cache.del('foo', function (err) {
  if (err) throw err;
  // foo was deleted
});
```

### cache.clear([fn])

Clear the cache entirely, throwing away all values.

```javascript
cache.clear(function (err) {
  if (err) throw err;
  // cache is now clear
});
```

## Run tests

``` bash
$ make test
```

## License

(The MIT License)

Copyright (c) 2015 Jonathan Brumley &lt;cayasso@gmail.com&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

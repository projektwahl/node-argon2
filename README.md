# node-argon2 [![NPM package][npm-image]][npm-url] [![Build status][codeship-image]][codeship-url] [![Coverage status][coverage-image]][coverage-url] [![Code Climate][codeclimate-image]][codeclimate-url] [![Dependencies][david-dm-image]][david-dm-url]
Bindings to the  reference [Argon2](https://github.com/P-H-C/phc-winner-argon2)
implementation.

### Before installing
You **MUST** have a **node-gyp** global install before proceeding with install.

### Usage
It's possible to hash a password using both Argon2i (default) and Argon2d, sync
and async, and to verify if a password matches a hash, and also generate random
cryptographically-safe salts.

To hash a password:
```js
var argon2 = require('argon2');

argon2.hash('password', 'somesalt', function (err, hash) {
  if (err) // hashion failure
    throw err;

  doSomethingWith(hash);
});

// OR

try {
  var hash = argon2.hashSync('password', 'somesalt');
} catch (err) {
  console.log(err);
}
```
Resultant hashes will be 90 characters long. You can choose between Argon2i and
Argon2d by passing an object as the third argument with the `argon2d` key set to
whether or not you want Argon2d:
```js
var argon2 = require('argon2');

argon2.hash('password', 'somesalt', {
  argon2d: true
}, function (err, hash) {
  // ...
});

// OR

try {
  var hash = argon2.hashSync('password', 'somesalt', {
    argon2d: true
  });
} catch (err) {
  // ...
}
```
The `argon2d` option is flexible and accepts any truthy or falsy values.

You can provide your own salt as the second parameter. It is recommended to use
the salt generating methods instead of a hardcoded, constant salt:
```js
var argon2 = require('argon2');

argon2.generateSalt(function (salt) {
  doSomethingWith(salt);
});

// OR

var salt = argon2.generateSaltSync();
```

You can also modify time, memory and parallelism constraints passing the object
as the third parameter, with keys `timeCost`, `memoryCost` and `parallelism`,
respectively defaulted to 3, 12 (meaning 2^12 KB) and 1 (threads):
```js
var argon2 = require('argon2');

argon2.generateSalt(function (salt) {
  argon2.hash('password', salt, {
    timeCost: 4, memoryCost: 13, parallelism: 2
  }, function (err, hash) {
    // ...
  });
});

// OR

var hash = argon2.hashSync('password', argon2.generateSalt(), {
  timeCost: 4, memoryCost: 13, parallelism: 2
});
```

The default parameters for Argon2 can be accessed with `defaults`:
```js
var argon2 = require('argon2');

console.log(argon2.defaults);
// => { timeCost: 3, memoryCost: 12, parallelism: 1, argon2d: false }
```

To verify a password:
```js
var argon2 = require('argon2');

argon2.verify('<big long hash>', 'password', function (err) {
  if (err) // password did not match
    throw err;

  authenticate();
});

// OR

if (argon2.verifySync('<big long hash>', 'password')) {
  authenticate();
} else {
  fail();
}
```
First parameter must have been generated by an Argon2 encoded hashing method,
not raw.

# License
Work licensed under the [MIT License](LICENSE). Please check
[P-H-C/phc-winner-argon2] (https://github.com/P-H-C/phc-winner-argon2) for
license over Argon2 and the reference implementation.

[npm-image]: https://img.shields.io/npm/v/argon2.svg?style=flat-square
[npm-url]: https://www.npmjs.com/package/argon2
[codeship-image]: https://img.shields.io/codeship/eb7243d0-f615-0132-1cce-020cea137f51/master.svg?style=flat-square
[codeship-url]: https://codeship.com/projects/85936
[coverage-image]: https://img.shields.io/coveralls/ranisalt/node-argon2/master.svg?style=flat-square
[coverage-url]: https://coveralls.io/github/ranisalt/node-argon2
[codeclimate-image]: https://img.shields.io/codeclimate/github/ranisalt/node-argon2.svg?style=flat-square
[codeclimate-url]: https://codeclimate.com/github/ranisalt/node-argon2
[david-dm-image]: https://img.shields.io/david/ranisalt/node-argon2.svg?style=flat-square
[david-dm-url]: https://david-dm.org/ranisalt/node-argon2

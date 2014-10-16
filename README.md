# mongo-bcrypt-user

Create user accounts, verify and update passwords using bcrypt. The library can be
used in a stateless way or in an object oriented way.

## Examples
### object oriented

Create a new user named "foo" in the "user" collection with the password "secr3t".

    var mongodb = require('mongodb');
    var User = require('mongo-bcrypt-user');

    // assume "db" is a mongodb.Db object
    var coll = db.collection('users');

    var user = new User(coll, 'foo');
    user.register('secr3t', function(err) {
      if (err) { throw err; }
      console.log('user created');
    });

Check if the password "raboof" is correct for user "foo" in the realm "bar".

    // same setup as previous example

    var user = new User(coll, 'foo', 'bar');
    user.verifyPassword('raboof', function(err, correct) {
      if (err) { throw err; }
      if (correct === true) {
        console.log('password correct');
      } else {
        console.log('password incorrect');
      }
    });

### stateless

Create a new user named "foo" in the "user" collection with the password "secr3t".

    var mongodb = require('mongodb');
    var User = require('mongo-bcrypt-user');

    // assume "db" is a mongodb.Db object
    var coll = db.collection('users');

    User.register(coll, 'foo', 'secr3t', function(err) {
      if (err) { throw err; }
      console.log('user created');
    });

Check if the password "raboof" is correct for user "foo" in the realm "bar".

    // same setup as previous example

    User.verifyPassword(coll, 'foo', 'raboof', 'bar', function(err, correct) {
      if (err) { throw err; }
      if (correct === true) {
        console.log('password correct');
      } else {
        console.log('password incorrect');
      }
    });

## Installation

    $ npm install mongo-bcrypt-user

## API
### object oriented

#### new User(coll, username, [realm])
* coll {mongodb.Collection} the database that contains all user accounts
* username {String} the name of the user to bind this instance to
* realm {String, default: _default} optional realm the user belongs to

Create a new User object. Either for maintenance, verification or registration.
A user may be bound to a realm.

#### user.exists(cb)
* cb {Function} first parameter will be an error or null, second parameter
  contains a boolean about whether this user exists or not.

Return whether or not the user already exists in the database.

#### user.verifyPassword(password, cb)
* password {String} the password to verify
* cb {Function} first parameter will be an error or null, second parameter
  contains a boolean about whether the password is valid or not.

Verify if the given password is valid.

#### user.setPassword(password, cb)
* password {String} the password to use
* cb {Function} first parameter will be either an error object or null on success.

Update the password.

Note: the user has to exist in the database.

#### user.register(password, cb)
* password {String} the password to use, at least 6 characters
* cb {Function} first parameter will be either an error object or null on success.

Register a new user with a certain password.

### stateless

Furthermore a stateless variant of each object oriented function is available
where the collection object, the username and optionally the realm are given at
each function invocation.

#### User.exists(coll, username, [realm], cb)
#### User.verifyPassword(coll, username, password, [realm], cb)
#### User.setPassword(coll, username, password, [realm], cb)
#### User.register(coll, username, password, [realm], cb)

## Tests

First make sure ./test/test.json has the right parameters. Then run `mocha test`.

## License

MIT, see LICENSE

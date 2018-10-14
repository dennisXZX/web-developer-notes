## Chai Assertion Library

#### Fundamentals

Chai comes with three different favors (assertion styles), this might confuse people with no automated testing background.

The `Expect/Should` style covers BDD (Behavior-driven development) assertion styles, while the `Assert` style deals with TDD (Test-driven development).

Assert style:

```js
const assert = require('chai').assert
    , foo = 'bar'
    , beverages = { tea: [ 'chai', 'matcha', 'oolong' ] };

assert.typeOf(foo, 'string'); // without optional message
assert.typeOf(foo, 'string', 'foo is a string'); // with optional message
assert.equal(foo, 'bar', 'foo equal `bar`');
assert.lengthOf(foo, 3, 'foo`s value has a length of 3');
assert.lengthOf(beverages.tea, 3, 'beverages has 3 types of tea');
```

Expect style:

```js
const expect = require('chai').expect
    , foo = 'bar'
    , beverages = { tea: [ 'chai', 'matcha', 'oolong' ] };

expect(foo).to.be.a('string');
expect(foo).to.equal('bar');
expect(foo).to.have.lengthOf(3);
expect(beverages).to.have.property('tea').with.lengthOf(3);
```

Should style:

```js
const should = require('chai').should() //actually call the function
    , foo = 'bar'
    , beverages = { tea: [ 'chai', 'matcha', 'oolong' ] };

foo.should.be.a('string');
foo.should.equal('bar');
foo.should.have.lengthOf(3);
beverages.should.have.property('tea').with.lengthOf(3);
```

Note: if you decide to go with BDD, you should choose `expect` over `should`. Because `should` works by extending `Object.prototype`, there are some caveats and also is not working well with Internet Explorer.

#### Expect style API

`.deep` is used to provide deep equality feature.
